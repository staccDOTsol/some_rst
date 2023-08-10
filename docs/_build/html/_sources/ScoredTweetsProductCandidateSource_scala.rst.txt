a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/candidate_source/ScoredTweetsProductCandidateSource.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/candidate_source/ScoredTweetsProductCandidateSource.scala
==================================================

Change Range: -128,15 +141,22

Content:

::

index db15a2cac..d1daeb93e 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/candidate_source/ScoredTweetsProductCandidateSource.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/candidate_source/ScoredTweetsProductCandidateSource.scala
  
   
  
   import com.google.inject.Provider
  
   import com.twitter.home_mixer.model.HomeFeatures.ServedTweetIdsFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.TimelineServiceTweetsFeature
  
   import com.twitter.home_mixer.model.request.HomeMixerRequest
  
   import com.twitter.home_mixer.model.request.ScoredTweetsProduct
  
   import com.twitter.home_mixer.model.request.ScoredTweetsProductContext
  
   import com.twitter.home_mixer.product.for_you.model.ForYouQuery
  
   import com.twitter.home_mixer.{thriftscala => t}
  
  +import com.twitter.product_mixer.component_library.premarshaller.cursor.UrtCursorSerializer
  
   import com.twitter.product_mixer.core.functional_component.candidate_source.product_pipeline.ProductPipelineCandidateSource
  
   import com.twitter.product_mixer.core.functional_component.configapi.ParamsBuilder
  
   import com.twitter.product_mixer.core.model.common.identifier.CandidateSourceIdentifier
  
     inReplyToUserId: Option[Long] = None,
  
     directedAtUserId: Option[Long] = None,
  
     inNetwork: Option[Boolean] = None,
  
  -  favoritedByUserIds: Option[Seq[Long]] = None,
  
  -  followedByUserIds: Option[Seq[Long]] = None,
  
  +  sgsValidLikedByUserIds: Option[Seq[Long]] = None,
  
  +  sgsValidFollowedByUserIds: Option[Seq[Long]] = None,
  
     ancestors: Option[Seq[ta.TweetAncestor]] = None,
  
     topicId: Option[Long] = None,
  
     topicFunctionalityType: Option[tl.TopicContextFunctionalityType] = None,
  
     conversationId: Option[Long] = None,
  
     conversationFocalTweetId: Option[Long] = None,
  
     isReadFromCache: Option[Boolean] = None,
  
  -  streamToKafka: Option[Boolean] = None)
  
  +  streamToKafka: Option[Boolean] = None,
  
  +  exclusiveConversationAuthorId: Option[Long] = None,
  
  +  authorIsBlueVerified: Option[Boolean] = None,
  
  +  authorIsGoldVerified: Option[Boolean] = None,
  
  +  authorIsGrayVerified: Option[Boolean] = None,
  
  +  authorIsLegacyVerified: Option[Boolean] = None,
  
  +  authorIsCreator: Option[Boolean] = None,
  
  +  perspectiveFilteredLikedByUserIds: Option[Seq[Long]] = None)
  
   
  
   @Singleton
  
   class ScoredTweetsProductCandidateSource @Inject() (
  
           ScoredTweetsProductContext(
  
             productPipelineQuery.deviceContext,
  
             productPipelineQuery.seenTweetIds,
  
  -          productPipelineQuery.features.map(_.getOrElse(ServedTweetIdsFeature, Seq.empty))
  
  +          productPipelineQuery.features.map(_.getOrElse(ServedTweetIdsFeature, Seq.empty)),
  
  +          productPipelineQuery.features.map(_.getOrElse(TimelineServiceTweetsFeature, Seq.empty))
  
           )),
  
  -      serializedRequestCursor = None,
  
  +      serializedRequestCursor =
  
  +        productPipelineQuery.pipelineCursor.map(UrtCursorSerializer.serializeCursor),
  
         maxResults = productPipelineQuery.requestedMaxResults,
  
         debugParams = None,
  
         homeRequestParam = false
  
             authorId = ancestor.userId,
  
             suggestType = focalTweet.suggestType,
  
             conversationId = Some(ancestor.tweetId),
  
  -          conversationFocalTweetId = Some(focalTweet.tweetId)
  
  +          conversationFocalTweetId = Some(focalTweet.tweetId),
  
  +          exclusiveConversationAuthorId = focalTweet.exclusiveConversationAuthorId
  
           )
  
         }
  
         val conversationId = rootScoredTweet.headOption.map(_.tweetId)
  
             suggestType = focalTweet.suggestType,
  
             inReplyToTweetId = tweetsToParents.get(ancestor).map(_.tweetId),
  
             conversationId = conversationId,
  
  -          conversationFocalTweetId = Some(focalTweet.tweetId)
  
  +          conversationFocalTweetId = Some(focalTweet.tweetId),
  
  +          exclusiveConversationAuthorId = focalTweet.exclusiveConversationAuthorId
  
           )
  
         }
  
         val parentScoredTweets = rootScoredTweet ++ intermediateScoredTweets
  
           inReplyToUserId = focalTweet.inReplyToUserId,
  
           directedAtUserId = focalTweet.directedAtUserId,
  
           inNetwork = focalTweet.inNetwork,
  
  -        favoritedByUserIds = focalTweet.favoritedByUserIds,
  
  -        followedByUserIds = focalTweet.followedByUserIds,
  
  +        sgsValidLikedByUserIds = focalTweet.sgsValidLikedByUserIds,
  
  +        sgsValidFollowedByUserIds = focalTweet.sgsValidFollowedByUserIds,
  
           topicId = focalTweet.topicId,
  
           topicFunctionalityType = focalTweet.topicFunctionalityType,
  
           ancestors = focalTweet.ancestors,
  
           conversationId = conversationId,
  
           conversationFocalTweetId = conversationFocalTweetId,
  
           isReadFromCache = focalTweet.isReadFromCache,
  
  -        streamToKafka = focalTweet.streamToKafka
  
  +        streamToKafka = focalTweet.streamToKafka,
  
  +        exclusiveConversationAuthorId = focalTweet.exclusiveConversationAuthorId,
  
  +        authorIsBlueVerified = focalTweet.authorMetadata.map(_.blueVerified),
  
  +        authorIsGoldVerified = focalTweet.authorMetadata.map(_.goldVerified),
  
  +        authorIsGrayVerified = focalTweet.authorMetadata.map(_.grayVerified),
  
  +        authorIsLegacyVerified = focalTweet.authorMetadata.map(_.legacyVerified),
  
  +        authorIsCreator = focalTweet.authorMetadata.map(_.creator),
  
  +        perspectiveFilteredLikedByUserIds = focalTweet.perspectiveFilteredLikedByUserIds
  
         )
  
   
  
         parentScoredTweets :+ focalScoredTweet
  
