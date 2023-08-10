a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/marshaller/ScoredTweetsResponseTransportMarshaller.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/marshaller/ScoredTweetsResponseTransportMarshaller.scala
==================================================

Change Range: -16,28 +19,52

Content:

::

index 355f076fe..27486768b 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/marshaller/ScoredTweetsResponseTransportMarshaller.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/marshaller/ScoredTweetsResponseTransportMarshaller.scala
  
   package com.twitter.home_mixer.product.scored_tweets.marshaller
  
   
  
  +import com.twitter.home_mixer.model.HomeFeatures._
  
   import com.twitter.home_mixer.product.scored_tweets.model.ScoredTweetsResponse
  
   import com.twitter.home_mixer.{thriftscala => t}
  
  +import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
   import com.twitter.product_mixer.core.functional_component.marshaller.TransportMarshaller
  
  +import com.twitter.product_mixer.core.functional_component.marshaller.response.urt.metadata.TopicContextFunctionalityTypeMarshaller
  
   import com.twitter.product_mixer.core.model.common.identifier.TransportMarshallerIdentifier
  
   
  
   /**
  
   
  
     override def apply(input: ScoredTweetsResponse): t.ScoredTweetsResponse = {
  
       val scoredTweets = input.scoredTweets.map { tweet =>
  
  -      t.ScoredTweet(
  
  -        tweetId = tweet.tweetId,
  
  -        authorId = tweet.authorId,
  
  -        score = tweet.score,
  
  -        suggestType = Some(tweet.suggestType),
  
  -        sourceTweetId = tweet.sourceTweetId,
  
  -        sourceUserId = tweet.sourceUserId,
  
  -        quotedTweetId = tweet.quotedTweetId,
  
  -        quotedUserId = tweet.quotedUserId,
  
  -        inReplyToTweetId = tweet.inReplyToTweetId,
  
  -        inReplyToUserId = tweet.inReplyToUserId,
  
  -        directedAtUserId = tweet.directedAtUserId,
  
  -        inNetwork = tweet.inNetwork,
  
  -        favoritedByUserIds = tweet.favoritedByUserIds,
  
  -        followedByUserIds = tweet.followedByUserIds,
  
  -        topicId = tweet.topicId,
  
  -        topicFunctionalityType = tweet.topicFunctionalityType,
  
  -        ancestors = tweet.ancestors,
  
  -        isReadFromCache = tweet.isReadFromCache,
  
  -        streamToKafka = tweet.streamToKafka
  
  -      )
  
  +      mkScoredTweet(tweet.candidateIdLong, tweet.features)
  
       }
  
       t.ScoredTweetsResponse(scoredTweets)
  
     }
  
  +
  
  +  private def mkScoredTweet(tweetId: Long, features: FeatureMap): t.ScoredTweet = {
  
  +    val topicFunctionalityType = features
  
  +      .getOrElse(TopicContextFunctionalityTypeFeature, None)
  
  +      .map(TopicContextFunctionalityTypeMarshaller(_))
  
  +
  
  +    t.ScoredTweet(
  
  +      tweetId = tweetId,
  
  +      authorId = features.get(AuthorIdFeature).get,
  
  +      score = features.get(ScoreFeature),
  
  +      suggestType = features.get(SuggestTypeFeature),
  
  +      sourceTweetId = features.getOrElse(SourceTweetIdFeature, None),
  
  +      sourceUserId = features.getOrElse(SourceUserIdFeature, None),
  
  +      quotedTweetId = features.getOrElse(QuotedTweetIdFeature, None),
  
  +      quotedUserId = features.getOrElse(QuotedUserIdFeature, None),
  
  +      inReplyToTweetId = features.getOrElse(InReplyToTweetIdFeature, None),
  
  +      inReplyToUserId = features.getOrElse(InReplyToUserIdFeature, None),
  
  +      directedAtUserId = features.getOrElse(DirectedAtUserIdFeature, None),
  
  +      inNetwork = Some(features.getOrElse(InNetworkFeature, true)),
  
  +      sgsValidLikedByUserIds = Some(features.getOrElse(SGSValidLikedByUserIdsFeature, Seq.empty)),
  
  +      sgsValidFollowedByUserIds =
  
  +        Some(features.getOrElse(SGSValidFollowedByUserIdsFeature, Seq.empty)),
  
  +      topicId = features.getOrElse(TopicIdSocialContextFeature, None),
  
  +      topicFunctionalityType = topicFunctionalityType,
  
  +      ancestors = Some(features.getOrElse(AncestorsFeature, Seq.empty)),
  
  +      isReadFromCache = Some(features.getOrElse(IsReadFromCacheFeature, false)),
  
  +      streamToKafka = Some(features.getOrElse(StreamToKafkaFeature, false)),
  
  +      exclusiveConversationAuthorId =
  
  +        features.getOrElse(ExclusiveConversationAuthorIdFeature, None),
  
  +      authorMetadata = Some(
  
  +        t.AuthorMetadata(
  
  +          blueVerified = features.getOrElse(AuthorIsBlueVerifiedFeature, false),
  
  +          goldVerified = features.getOrElse(AuthorIsGoldVerifiedFeature, false),
  
  +          grayVerified = features.getOrElse(AuthorIsGrayVerifiedFeature, false),
  
  +          legacyVerified = features.getOrElse(AuthorIsLegacyVerifiedFeature, false),
  
  +          creator = features.getOrElse(AuthorIsCreatorFeature, false)
  
  +        )),
  
  +      lastScoredTimestampMs = None,
  
  +      candidatePipelineIdentifier = None,
  
  +      tweetUrls = None,
  
  +      perspectiveFilteredLikedByUserIds =
  
  +        Some(features.getOrElse(PerspectiveFilteredLikedByUserIdsFeature, Seq.empty)),
  
  +    )
  
  +  }
  
   }
  
