a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/response_transformer/CachedScoredTweetsResponseFeatureTransformer.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/response_transformer/CachedScoredTweetsResponseFeatureTransformer.scala
==================================================

Change Range: -63,32 +77,43

Content:

::

index 9822a5331..0fbe7a438 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/response_transformer/CachedScoredTweetsResponseFeatureTransformer.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/response_transformer/CachedScoredTweetsResponseFeatureTransformer.scala
  
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
  
   import com.twitter.home_mixer.model.HomeFeatures.IsReadFromCacheFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.IsRetweetFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.LastScoredTimestampMsFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.PerspectiveFilteredLikedByUserIdsFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.QuotedTweetIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.QuotedUserIdFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.SGSValidFollowedByUserIdsFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.SGSValidLikedByUserIdsFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.ScoreFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.SourceTweetIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.SourceUserIdFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.StreamToKafkaFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.SuggestTypeFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.TopicContextFunctionalityTypeFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.TopicIdSocialContextFeature
  
   import com.twitter.product_mixer.core.model.common.identifier.TransformerIdentifier
  
   
  
   object CachedScoredTweetsResponseFeatureTransformer
  
  -    extends CandidateFeatureTransformer[hmt.CachedScoredTweet] {
  
  +    extends CandidateFeatureTransformer[hmt.ScoredTweet] {
  
   
  
     override val identifier: TransformerIdentifier =
  
       TransformerIdentifier("CachedScoredTweetsResponse")
  
       AncestorsFeature,
  
       AuthorIdFeature,
  
       AuthorIsBlueVerifiedFeature,
  
  +    AuthorIsCreatorFeature,
  
  +    AuthorIsGoldVerifiedFeature,
  
  +    AuthorIsGrayVerifiedFeature,
  
  +    AuthorIsLegacyVerifiedFeature,
  
       CachedCandidatePipelineIdentifierFeature,
  
       DirectedAtUserIdFeature,
  
  -    FavoritedByUserIdsFeature,
  
  -    FollowedByUserIdsFeature,
  
  +    ExclusiveConversationAuthorIdFeature,
  
       InNetworkFeature,
  
       InReplyToTweetIdFeature,
  
       InReplyToUserIdFeature,
  
       IsReadFromCacheFeature,
  
       IsRetweetFeature,
  
       LastScoredTimestampMsFeature,
  
  +    PerspectiveFilteredLikedByUserIdsFeature,
  
       QuotedTweetIdFeature,
  
       QuotedUserIdFeature,
  
  +    SGSValidFollowedByUserIdsFeature,
  
  +    SGSValidLikedByUserIdsFeature,
  
       ScoreFeature,
  
       SourceTweetIdFeature,
  
       SourceUserIdFeature,
  
  +    StreamToKafkaFeature,
  
       SuggestTypeFeature,
  
       TopicContextFunctionalityTypeFeature,
  
       TopicIdSocialContextFeature,
  
       WeightedModelScoreFeature
  
     )
  
   
  
  -  override def transform(candidate: hmt.CachedScoredTweet): FeatureMap =
  
  +  override def transform(candidate: hmt.ScoredTweet): FeatureMap =
  
       FeatureMapBuilder()
  
         .add(AncestorsFeature, candidate.ancestors.getOrElse(Seq.empty))
  
  -      .add(AuthorIdFeature, candidate.userId)
  
  -      .add(AuthorIsBlueVerifiedFeature, candidate.authorIsBlueVerified.getOrElse(false))
  
  +      .add(AuthorIdFeature, Some(candidate.authorId))
  
  +      .add(AuthorIsBlueVerifiedFeature, candidate.authorMetadata.exists(_.blueVerified))
  
  +      .add(AuthorIsGoldVerifiedFeature, candidate.authorMetadata.exists(_.goldVerified))
  
  +      .add(AuthorIsGrayVerifiedFeature, candidate.authorMetadata.exists(_.grayVerified))
  
  +      .add(AuthorIsLegacyVerifiedFeature, candidate.authorMetadata.exists(_.legacyVerified))
  
  +      .add(AuthorIsCreatorFeature, candidate.authorMetadata.exists(_.creator))
  
         .add(CachedCandidatePipelineIdentifierFeature, candidate.candidatePipelineIdentifier)
  
         .add(DirectedAtUserIdFeature, candidate.directedAtUserId)
  
  -      .add(FavoritedByUserIdsFeature, candidate.favoritedByUserIds.getOrElse(Seq.empty))
  
  -      .add(FollowedByUserIdsFeature, candidate.followedByUserIds.getOrElse(Seq.empty))
  
  -      .add(InNetworkFeature, candidate.isInNetwork.getOrElse(false))
  
  +      .add(ExclusiveConversationAuthorIdFeature, candidate.exclusiveConversationAuthorId)
  
  +      .add(InNetworkFeature, candidate.inNetwork.getOrElse(true))
  
         .add(InReplyToTweetIdFeature, candidate.inReplyToTweetId)
  
         .add(InReplyToUserIdFeature, candidate.inReplyToUserId)
  
         .add(IsReadFromCacheFeature, true)
  
  -      .add(IsRetweetFeature, candidate.isRetweet.getOrElse(false))
  
  +      .add(IsRetweetFeature, candidate.sourceTweetId.isDefined)
  
         .add(LastScoredTimestampMsFeature, candidate.lastScoredTimestampMs)
  
  +      .add(
  
  +        PerspectiveFilteredLikedByUserIdsFeature,
  
  +        candidate.perspectiveFilteredLikedByUserIds.getOrElse(Seq.empty))
  
         .add(QuotedTweetIdFeature, candidate.quotedTweetId)
  
         .add(QuotedUserIdFeature, candidate.quotedUserId)
  
         .add(ScoreFeature, candidate.score)
  
  +      .add(SGSValidLikedByUserIdsFeature, candidate.sgsValidLikedByUserIds.getOrElse(Seq.empty))
  
  +      .add(
  
  +        SGSValidFollowedByUserIdsFeature,
  
  +        candidate.sgsValidFollowedByUserIds.getOrElse(Seq.empty))
  
         .add(SourceTweetIdFeature, candidate.sourceTweetId)
  
         .add(SourceUserIdFeature, candidate.sourceUserId)
  
  +      .add(StreamToKafkaFeature, false)
  
         .add(SuggestTypeFeature, candidate.suggestType)
  
         .add(
  
           TopicContextFunctionalityTypeFeature,
  
           candidate.topicFunctionalityType.map(TopicContextFunctionalityTypeUnmarshaller(_)))
  
         .add(TopicIdSocialContextFeature, candidate.topicId)
  
  -      .add(TweetUrlsFeature, candidate.urlsList.getOrElse(Seq.empty))
  
  +      .add(TweetUrlsFeature, candidate.tweetUrls.getOrElse(Seq.empty))
  
         .add(WeightedModelScoreFeature, candidate.score)
  
         .build()
  
   }
  
