a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/side_effect/CachedScoredTweetsSideEffect.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/side_effect/CachedScoredTweetsSideEffect.scala
==================================================

Change Range: -48,71 +51,79

Content:

::

index abe076c54..3d66ff54a 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/side_effect/CachedScoredTweetsSideEffect.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/side_effect/CachedScoredTweetsSideEffect.scala
  
   import com.twitter.home_mixer.model.HomeFeatures.AncestorsFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.AuthorIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.AuthorIsBlueVerifiedFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.AuthorIsCreatorFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.AuthorIsGoldVerifiedFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.AuthorIsGrayVerifiedFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.AuthorIsLegacyVerifiedFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.CachedCandidatePipelineIdentifierFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.DirectedAtUserIdFeature
  
  -import com.twitter.home_mixer.model.HomeFeatures.FavoritedByUserIdsFeature
  
  -import com.twitter.home_mixer.model.HomeFeatures.FollowedByUserIdsFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.ExclusiveConversationAuthorIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.InNetworkFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.InReplyToTweetIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.InReplyToUserIdFeature
  
  -import com.twitter.home_mixer.model.HomeFeatures.IsRetweetFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.LastScoredTimestampMsFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.PerspectiveFilteredLikedByUserIdsFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.QuotedTweetIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.QuotedUserIdFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.SGSValidFollowedByUserIdsFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.SGSValidLikedByUserIdsFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.ScoreFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.SourceTweetIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.SourceUserIdFeature
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
   import com.twitter.servo.cache.TtlCache
  
   import com.twitter.stitch.Stitch
  
  -import com.twitter.util.Time
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
   @Singleton
  
   class CachedScoredTweetsSideEffect @Inject() (
  
  -  scoredTweetsCache: TtlCache[Long, hmt.CachedScoredTweets])
  
  +  scoredTweetsCache: TtlCache[Long, hmt.ScoredTweetsResponse])
  
       extends PipelineResultSideEffect[PipelineQuery, ScoredTweetsResponse] {
  
   
  
     override val identifier: SideEffectIdentifier = SideEffectIdentifier("CachedScoredTweets")
  
     private val MaxTweetsToCache = 1000
  
   
  
     def buildCachedScoredTweets(
  
  +    query: PipelineQuery,
  
       candidates: Seq[CandidateWithDetails]
  
  -  ): hmt.CachedScoredTweets = {
  
  +  ): hmt.ScoredTweetsResponse = {
  
       val tweets = candidates.map { candidate =>
  
  -      val favoritedByUserIds = candidate.features.getOrElse(FavoritedByUserIdsFeature, Seq.empty)
  
  -      val followedByUserIds = candidate.features.getOrElse(FollowedByUserIdsFeature, Seq.empty)
  
  +      val sgsValidLikedByUserIds =
  
  +        candidate.features.getOrElse(SGSValidLikedByUserIdsFeature, Seq.empty)
  
  +      val sgsValidFollowedByUserIds =
  
  +        candidate.features.getOrElse(SGSValidFollowedByUserIdsFeature, Seq.empty)
  
  +      val perspectiveFilteredLikedByUserIds =
  
  +        candidate.features.getOrElse(PerspectiveFilteredLikedByUserIdsFeature, Seq.empty)
  
         val ancestors = candidate.features.getOrElse(AncestorsFeature, Seq.empty)
  
  -      val urlsList = candidate.features.getOrElse(TweetUrlsFeature, Seq.empty)
  
   
  
  -      hmt.CachedScoredTweet(
  
  +      hmt.ScoredTweet(
  
           tweetId = candidate.candidateIdLong,
  
  +        authorId = candidate.features.get(AuthorIdFeature).get,
  
           // Cache the model score instead of the final score because rescoring is per-request
  
           score = candidate.features.getOrElse(WeightedModelScoreFeature, None),
  
  -        lastScoredTimestampMs = Some(
  
  -          candidate.features
  
  -            .getOrElse(LastScoredTimestampMsFeature, None)
  
  -            .getOrElse(Time.now.inMilliseconds)),
  
  -        candidatePipelineIdentifier = Some(
  
  -          candidate.features
  
  -            .getOrElse(CachedCandidatePipelineIdentifierFeature, None)
  
  -            .getOrElse(candidate.source.name)),
  
  -        userId = candidate.features.getOrElse(AuthorIdFeature, None),
  
  +        suggestType = candidate.features.getOrElse(SuggestTypeFeature, None),
  
           sourceTweetId = candidate.features.getOrElse(SourceTweetIdFeature, None),
  
           sourceUserId = candidate.features.getOrElse(SourceUserIdFeature, None),
  
  -        isRetweet = Some(candidate.features.getOrElse(IsRetweetFeature, false)),
  
  -        isInNetwork = Some(candidate.features.getOrElse(InNetworkFeature, false)),
  
  -        suggestType = candidate.features.getOrElse(SuggestTypeFeature, None),
  
           quotedTweetId = candidate.features.getOrElse(QuotedTweetIdFeature, None),
  
           quotedUserId = candidate.features.getOrElse(QuotedUserIdFeature, None),
  
           inReplyToTweetId = candidate.features.getOrElse(InReplyToTweetIdFeature, None),
  
           inReplyToUserId = candidate.features.getOrElse(InReplyToUserIdFeature, None),
  
           directedAtUserId = candidate.features.getOrElse(DirectedAtUserIdFeature, None),
  
  -        favoritedByUserIds = if (favoritedByUserIds.nonEmpty) Some(favoritedByUserIds) else None,
  
  -        followedByUserIds = if (followedByUserIds.nonEmpty) Some(followedByUserIds) else None,
  
  +        inNetwork = Some(candidate.features.getOrElse(InNetworkFeature, true)),
  
  +        sgsValidLikedByUserIds = Some(sgsValidLikedByUserIds),
  
  +        sgsValidFollowedByUserIds = Some(sgsValidFollowedByUserIds),
  
           topicId = candidate.features.getOrElse(TopicIdSocialContextFeature, None),
  
           topicFunctionalityType = candidate.features
  
             .getOrElse(TopicContextFunctionalityTypeFeature, None).map(
  
               TopicContextFunctionalityTypeMarshaller(_)),
  
           ancestors = if (ancestors.nonEmpty) Some(ancestors) else None,
  
  -        urlsList = if (urlsList.nonEmpty) Some(urlsList) else None,
  
  -        authorIsBlueVerified =
  
  -          Some(candidate.features.getOrElse(AuthorIsBlueVerifiedFeature, false))
  
  +        isReadFromCache = Some(true),
  
  +        streamToKafka = Some(false),
  
  +        exclusiveConversationAuthorId = candidate.features
  
  +          .getOrElse(ExclusiveConversationAuthorIdFeature, None),
  
  +        authorMetadata = Some(
  
  +          hmt.AuthorMetadata(
  
  +            blueVerified = candidate.features.getOrElse(AuthorIsBlueVerifiedFeature, false),
  
  +            goldVerified = candidate.features.getOrElse(AuthorIsGoldVerifiedFeature, false),
  
  +            grayVerified = candidate.features.getOrElse(AuthorIsGrayVerifiedFeature, false),
  
  +            legacyVerified = candidate.features.getOrElse(AuthorIsLegacyVerifiedFeature, false),
  
  +            creator = candidate.features.getOrElse(AuthorIsCreatorFeature, false)
  
  +          )),
  
  +        lastScoredTimestampMs = candidate.features
  
  +          .getOrElse(LastScoredTimestampMsFeature, Some(query.queryTime.inMilliseconds)),
  
  +        candidatePipelineIdentifier = candidate.features
  
  +          .getOrElse(CachedCandidatePipelineIdentifierFeature, Some(candidate.source.name)),
  
  +        tweetUrls = Some(candidate.features.getOrElse(TweetUrlsFeature, Seq.empty)),
  
  +        perspectiveFilteredLikedByUserIds = Some(perspectiveFilteredLikedByUserIds)
  
         )
  
       }
  
   
  
  -    hmt.CachedScoredTweets(tweets = tweets)
  
  +    hmt.ScoredTweetsResponse(tweets)
  
     }
  
   
  
     final override def apply(
  
       inputs: PipelineResultSideEffect.Inputs[PipelineQuery, ScoredTweetsResponse]
  
     ): Stitch[Unit] = {
  
  -    val candidates = (inputs.selectedCandidates ++ inputs.remainingCandidates).filter { candidate =>
  
  -      val score = candidate.features.getOrElse(ScoreFeature, None)
  
  -      score.exists(_ > 0.0)
  
  -    }
  
  +    val candidates =
  
  +      (inputs.selectedCandidates ++ inputs.remainingCandidates ++ inputs.droppedCandidates)
  
  +        .filter(_.features.getOrElse(ScoreFeature, None).exists(_ > 0.0))
  
   
  
       val truncatedCandidates =
  
         if (candidates.size > MaxTweetsToCache)
  
           candidates
  
  -          .sortBy(-_.features.getOrElse(ScoreFeature, None).getOrElse(0.0))
  
  -          .take(MaxTweetsToCache)
  
  +          .sortBy(-_.features.getOrElse(ScoreFeature, None).getOrElse(0.0)).take(MaxTweetsToCache)
  
         else candidates
  
   
  
       if (truncatedCandidates.nonEmpty) {
  
         val ttl = inputs.query.params(CachedScoredTweets.TTLParam)
  
  -      val scoredTweets = buildCachedScoredTweets(truncatedCandidates)
  
  +      val scoredTweets = buildCachedScoredTweets(inputs.query, truncatedCandidates)
  
         Stitch.callFuture(scoredTweetsCache.set(inputs.query.getRequiredUserId, scoredTweets, ttl))
  
       } else Stitch.Unit
  
     }
  
