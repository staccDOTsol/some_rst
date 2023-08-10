a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouTimelineScorerCandidatePipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouTimelineScorerCandidatePipelineConfig.scala
==================================================

Change Range: -191,6 +194,7

Content:

::

index 37f93c2a1..bb93a4110 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouTimelineScorerCandidatePipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouTimelineScorerCandidatePipelineConfig.scala
  
   package com.twitter.home_mixer.product.for_you
  
   
  
  -import com.twitter.home_mixer.functional_component.decorator.HomeFeedbackActionInfoBuilder
  
  -import com.twitter.home_mixer.functional_component.decorator.HomeTimelinesScoreInfoBuilder
  
  -import com.twitter.home_mixer.functional_component.decorator.HomeTweetSocialContextBuilder
  
   import com.twitter.home_mixer.functional_component.decorator.builder.HomeClientEventInfoBuilder
  
   import com.twitter.home_mixer.functional_component.decorator.builder.HomeConversationModuleMetadataBuilder
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.FocalTweetFeatureHydrator
  
  +import com.twitter.home_mixer.functional_component.decorator.builder.HomeTimelinesScoreInfoBuilder
  
  +import com.twitter.home_mixer.functional_component.decorator.urt.builder.HomeFeedbackActionInfoBuilder
  
  +import com.twitter.home_mixer.functional_component.decorator.urt.builder.HomeTweetSocialContextBuilder
  
  +import com.twitter.home_mixer.functional_component.feature_hydrator.InNetworkFeatureHydrator
  
   import com.twitter.home_mixer.functional_component.feature_hydrator.NamesFeatureHydrator
  
   import com.twitter.home_mixer.functional_component.feature_hydrator.PerspectiveFilteredSocialContextFeatureHydrator
  
   import com.twitter.home_mixer.functional_component.feature_hydrator.SGSValidSocialContextFeatureHydrator
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.SocialGraphServiceFeatureHydrator
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.TimelineServiceTweetsFeature
  
   import com.twitter.home_mixer.functional_component.feature_hydrator.TweetypieFeatureHydrator
  
   import com.twitter.home_mixer.functional_component.filter.FeedbackFatigueFilter
  
   import com.twitter.home_mixer.functional_component.filter.InvalidConversationModuleFilter
  
  -import com.twitter.home_mixer.functional_component.filter.PredicateFeatureFilter
  
  +import com.twitter.home_mixer.functional_component.filter.InvalidSubscriptionTweetFilter
  
   import com.twitter.home_mixer.functional_component.filter.RejectTweetFromViewerFilter
  
   import com.twitter.home_mixer.functional_component.filter.RetweetDeduplicationFilter
  
  -import com.twitter.home_mixer.functional_component.filter.SocialContextFilter
  
   import com.twitter.home_mixer.functional_component.scorer.FeedbackFatigueScorer
  
   import com.twitter.home_mixer.functional_component.scorer.OONTweetScalingScorer
  
   import com.twitter.home_mixer.marshaller.timelines.DeviceContextMarshaller
  
   import com.twitter.home_mixer.model.HomeFeatures.IsHydratedFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.IsNsfwFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.QuotedTweetDroppedFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.TimelineServiceTweetsFeature
  
   import com.twitter.home_mixer.model.request.DeviceContext
  
  +import com.twitter.home_mixer.product.for_you.feature_hydrator.FocalTweetFeatureHydrator
  
  +import com.twitter.home_mixer.product.for_you.filter.SocialContextFilter
  
   import com.twitter.home_mixer.product.for_you.model.ForYouQuery
  
   import com.twitter.home_mixer.product.for_you.param.ForYouParam.EnableTimelineScorerCandidatePipelineParam
  
   import com.twitter.home_mixer.service.HomeMixerAlertConfig
  
   import com.twitter.product_mixer.component_library.decorator.urt.builder.timeline_module.StaticModuleDisplayTypeBuilder
  
   import com.twitter.product_mixer.component_library.decorator.urt.builder.timeline_module.TimelineModuleBuilder
  
   import com.twitter.product_mixer.component_library.filter.FeatureFilter
  
  +import com.twitter.product_mixer.component_library.filter.PredicateFeatureFilter
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.functional_component.candidate_source.BaseCandidateSource
  
   import com.twitter.product_mixer.core.functional_component.common.alert.Alert
  
     timelineScorerCandidateSource: TimelineScorerCandidateSource,
  
     deviceContextMarshaller: DeviceContextMarshaller,
  
     tweetypieFeatureHydrator: TweetypieFeatureHydrator,
  
  -  sgsFeatureHydrator: SocialGraphServiceFeatureHydrator,
  
     sgsValidSocialContextFeatureHydrator: SGSValidSocialContextFeatureHydrator,
  
     perspectiveFilteredSocialContextFeatureHydrator: PerspectiveFilteredSocialContextFeatureHydrator,
  
     namesFeatureHydrator: NamesFeatureHydrator,
  
     focalTweetFeatureHydrator: FocalTweetFeatureHydrator,
  
  +  invalidSubscriptionTweetFilter: InvalidSubscriptionTweetFilter,
  
     homeFeedbackActionInfoBuilder: HomeFeedbackActionInfoBuilder,
  
     homeTweetSocialContextBuilder: HomeTweetSocialContextBuilder)
  
       extends CandidatePipelineConfig[
  
         CandidateTweetSourceId.CroonTweet,
  
         CandidateTweetSourceId.RecommendedTweet,
  
         CandidateTweetSourceId.FrsTweet,
  
  -      CandidateTweetSourceId.ListTweet
  
  +      CandidateTweetSourceId.ListTweet,
  
  +      CandidateTweetSourceId.RecommendedTrendTweet,
  
  +      CandidateTweetSourceId.PopularTopicTweet
  
       )
  
   
  
       val timelineServiceTweets =
  
     override val preFilterFeatureHydrationPhase1: Seq[
  
       BaseCandidateFeatureHydrator[ForYouQuery, TweetCandidate, _]
  
     ] = Seq(
  
  +    namesFeatureHydrator,
  
       tweetypieFeatureHydrator,
  
  -    sgsFeatureHydrator,
  
  +    InNetworkFeatureHydrator,
  
       sgsValidSocialContextFeatureHydrator,
  
       perspectiveFilteredSocialContextFeatureHydrator,
  
  -    namesFeatureHydrator
  
     )
  
   
  
     override def filters: Seq[Filter[ForYouQuery, TweetCandidate]] = Seq(
  
       FeedbackFatigueFilter,
  
       RejectTweetFromViewerFilter,
  
       SocialContextFilter,
  
  +    invalidSubscriptionTweetFilter,
  
       InvalidConversationModuleFilter
  
     )
  
   
  
