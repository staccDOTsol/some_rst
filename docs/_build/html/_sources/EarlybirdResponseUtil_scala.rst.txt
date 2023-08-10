a/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/earlybird/EarlybirdResponseUtil.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/earlybird/EarlybirdResponseUtil.scala
==================================================

Change Range: -362,6 +397,13

Content:

::

index fb52e9689..06e0dd708 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/earlybird/EarlybirdResponseUtil.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/earlybird/EarlybirdResponseUtil.scala
  
   import com.twitter.search.common.schema.earlybird.EarlybirdFieldConstants.EarlybirdFieldConstant._
  
   import com.twitter.search.common.util.lang.ThriftLanguageUtil
  
   import com.twitter.search.earlybird.{thriftscala => eb}
  
  +import com.twitter.timelines.earlybird.common.utils.InNetworkEngagement
  
   
  
   object EarlybirdResponseUtil {
  
   
  
       facetLabels.filter(_.fieldName == facetName).map(_.label.filterNot(charsToRemove))
  
     }
  
   
  
  +  private def isUserMentioned(
  
  +    screenName: Option[String],
  
  +    mentions: Seq[String],
  
  +    mentionsInSourceTweet: Seq[String]
  
  +  ): Boolean =
  
  +    isUserMentioned(screenName, mentions) || isUserMentioned(screenName, mentionsInSourceTweet)
  
  +
  
     private def isUserMentioned(
  
       screenName: Option[String],
  
       mentions: Seq[String]
  
         None
  
     }
  
   
  
  -  def getOONTweetThriftFeaturesByTweetId(
  
  +  def getTweetThriftFeaturesByTweetId(
  
       searcherUserId: Long,
  
       screenName: Option[String],
  
       userLanguages: Seq[scc.ThriftLanguage],
  
       uiLanguageCode: Option[String] = None,
  
  +    followedUserIds: Set[Long],
  
  +    mutuallyFollowingUserIds: Set[Long],
  
       searchResults: Seq[eb.ThriftSearchResult],
  
  +    sourceTweetSearchResults: Seq[eb.ThriftSearchResult],
  
     ): Map[Long, sc.ThriftTweetFeatures] = {
  
   
  
  +    val allSearchResults = searchResults ++ sourceTweetSearchResults
  
  +    val sourceTweetSearchResultById =
  
  +      sourceTweetSearchResults.map(result => (result.id -> result)).toMap
  
  +    val inNetworkEngagement =
  
  +      InNetworkEngagement(followedUserIds.toSeq, mutuallyFollowingUserIds, allSearchResults)
  
       searchResults.map { searchResult =>
  
  -      val features = getOONThriftTweetFeaturesFromSearchResult(
  
  +      val features = getThriftTweetFeaturesFromSearchResult(
  
           searcherUserId,
  
           screenName,
  
           userLanguages,
  
           getLanguage(uiLanguageCode),
  
           getTweetCountByAuthorId(searchResults),
  
  +        followedUserIds,
  
  +        mutuallyFollowingUserIds,
  
  +        sourceTweetSearchResultById,
  
  +        inNetworkEngagement,
  
           searchResult
  
         )
  
         (searchResult.id -> features)
  
       }.toMap
  
     }
  
   
  
  -  private[earlybird] def getOONThriftTweetFeaturesFromSearchResult(
  
  +  private[earlybird] def getThriftTweetFeaturesFromSearchResult(
  
       searcherUserId: Long,
  
       screenName: Option[String],
  
       userLanguages: Seq[scc.ThriftLanguage],
  
       uiLanguage: Option[scc.ThriftLanguage],
  
       tweetCountByAuthorId: Map[Long, Int],
  
  -    searchResult: eb.ThriftSearchResult
  
  +    followedUserIds: Set[Long],
  
  +    mutuallyFollowingUserIds: Set[Long],
  
  +    sourceTweetSearchResultById: Map[Long, eb.ThriftSearchResult],
  
  +    inNetworkEngagement: InNetworkEngagement,
  
  +    searchResult: eb.ThriftSearchResult,
  
     ): sc.ThriftTweetFeatures = {
  
       val applyFeatures = (applyUserIndependentFeatures(
  
         searchResult
  
       )(_)).andThen(
  
  -      applyOONUserDependentFeatures(
  
  +      applyUserDependentFeatures(
  
           searcherUserId,
  
           screenName,
  
           userLanguages,
  
           uiLanguage,
  
           tweetCountByAuthorId,
  
  +        followedUserIds,
  
  +        mutuallyFollowingUserIds,
  
  +        sourceTweetSearchResultById,
  
  +        inNetworkEngagement,
  
           searchResult
  
         )(_)
  
       )
  
         }
  
         .getOrElse(thriftTweetFeatures)
  
   
  
  -    if (result.tweetSource.contains(eb.ThriftTweetSource.RealtimeProtectedCluster)) {
  
  -      features.copy(isProtected = true)
  
  -    } else {
  
  -      features
  
  -    }
  
  +    features
  
     }
  
   
  
  -  // Omitting inNetwork features e.g source tweet features and follow graph.
  
  -  // Can be expanded to include InNetwork in the future.
  
  -  def applyOONUserDependentFeatures(
  
  +  private def applyUserDependentFeatures(
  
       searcherUserId: Long,
  
       screenName: Option[String],
  
       userLanguages: Seq[scc.ThriftLanguage],
  
       uiLanguage: Option[scc.ThriftLanguage],
  
       tweetCountByAuthorId: Map[Long, Int],
  
  +    followedUserIds: Set[Long],
  
  +    mutuallyFollowingUserIds: Set[Long],
  
  +    sourceTweetSearchResultById: Map[Long, eb.ThriftSearchResult],
  
  +    inNetworkEngagement: InNetworkEngagement,
  
       result: eb.ThriftSearchResult
  
     )(
  
       thriftTweetFeatures: sc.ThriftTweetFeatures
  
       result.metadata
  
         .map { metadata =>
  
           val isRetweet = metadata.isRetweet.getOrElse(false)
  
  +        val sourceTweet =
  
  +          if (isRetweet) sourceTweetSearchResultById.get(metadata.sharedStatusId)
  
  +          else None
  
  +        val mentionsInSourceTweet = sourceTweet.map(getMentions).getOrElse(Seq.empty)
  
  +
  
           val isReply = metadata.isReply.getOrElse(false)
  
           val replyToSearcher = isReply && (metadata.referencedTweetAuthorId == searcherUserId)
  
           val replyOther = isReply && !replyToSearcher
  
           val retweetOther = isRetweet && (metadata.referencedTweetAuthorId != searcherUserId)
  
           val tweetLanguage = metadata.language.getOrElse(scc.ThriftLanguage.Unknown)
  
   
  
  +        val referencedTweetAuthorId =
  
  +          if (metadata.referencedTweetAuthorId > 0) Some(metadata.referencedTweetAuthorId) else None
  
  +        val inReplyToUserId = if (!isRetweet) referencedTweetAuthorId else None
  
  +
  
           thriftTweetFeatures.copy(
  
             // Info about the Tweet.
  
             fromSearcher = metadata.fromUserId == searcherUserId,
  
  -          probablyFromFollowedAuthor = false,
  
  -          fromMutualFollow = false,
  
  +          probablyFromFollowedAuthor = followedUserIds.contains(metadata.fromUserId),
  
  +          fromMutualFollow = mutuallyFollowingUserIds.contains(metadata.fromUserId),
  
             replySearcher = replyToSearcher,
  
             replyOther = replyOther,
  
             retweetOther = retweetOther,
  
  -          mentionSearcher = isUserMentioned(screenName, getMentions(result)),
  
  +          mentionSearcher = isUserMentioned(screenName, getMentions(result), mentionsInSourceTweet),
  
             // Info about Tweet content/media.
  
             matchesSearcherMainLang = isUsersMainLanguage(tweetLanguage, userLanguages),
  
             matchesSearcherLangs = isUsersLanguage(tweetLanguage, userLanguages),
  
             prevUserTweetEngagement =
  
               metadata.extraMetadata.flatMap(_.prevUserTweetEngagement).getOrElse(DefaultCount),
  
             tweetCountFromUserInSnapshot = tweetCountByAuthorId(metadata.fromUserId),
  
  +          bidirectionalReplyCount = inNetworkEngagement.biDirectionalReplyCounts(result.id),
  
  +          unidirectionalReplyCount = inNetworkEngagement.uniDirectionalReplyCounts(result.id),
  
  +          bidirectionalRetweetCount = inNetworkEngagement.biDirectionalRetweetCounts(result.id),
  
  +          unidirectionalRetweetCount = inNetworkEngagement.uniDirectionalRetweetCounts(result.id),
  
  +          conversationCount = inNetworkEngagement.descendantReplyCounts(result.id),
  
  +          directedAtUserIdIsInFirstDegree =
  
  +            if (isReply) inReplyToUserId.map(followedUserIds.contains) else None,
  
           )
  
         }
  
         .getOrElse(thriftTweetFeatures)
  
