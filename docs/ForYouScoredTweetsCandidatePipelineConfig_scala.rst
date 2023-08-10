a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouScoredTweetsCandidatePipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouScoredTweetsCandidatePipelineConfig.scala
==================================================

Change Range: -128,8 +119,6

Content:

::

index a8e2c36da..d7716e190 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouScoredTweetsCandidatePipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouScoredTweetsCandidatePipelineConfig.scala
  
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
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.PerspectiveFilteredSocialContextFeatureHydrator
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.SGSValidSocialContextFeatureHydrator
  
   import com.twitter.home_mixer.functional_component.feature_hydrator.TweetypieFeatureHydrator
  
  -import com.twitter.home_mixer.functional_component.filter.FeedbackFatigueFilter
  
   import com.twitter.home_mixer.functional_component.filter.InvalidConversationModuleFilter
  
  -import com.twitter.home_mixer.functional_component.filter.PredicateFeatureFilter
  
  -import com.twitter.home_mixer.functional_component.filter.SocialContextFilter
  
  +import com.twitter.home_mixer.functional_component.filter.InvalidSubscriptionTweetFilter
  
   import com.twitter.home_mixer.functional_component.gate.SupportedLanguagesGate
  
  -import com.twitter.home_mixer.functional_component.scorer.FeedbackFatigueScorer
  
   import com.twitter.home_mixer.model.HomeFeatures.ConversationModuleFocalTweetIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.InNetworkFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.IsHydratedFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.QuotedTweetDroppedFeature
  
   import com.twitter.home_mixer.product.for_you.candidate_source.ScoredTweetWithConversationMetadata
  
   import com.twitter.home_mixer.product.for_you.candidate_source.ScoredTweetsProductCandidateSource
  
  +import com.twitter.home_mixer.product.for_you.feature_hydrator.FocalTweetFeatureHydrator
  
  +import com.twitter.home_mixer.product.for_you.filter.SocialContextFilter
  
   import com.twitter.home_mixer.product.for_you.model.ForYouQuery
  
   import com.twitter.home_mixer.product.for_you.param.ForYouParam.EnableScoredTweetsCandidatePipelineParam
  
   import com.twitter.home_mixer.service.HomeMixerAlertConfig
  
   import com.twitter.product_mixer.component_library.decorator.urt.builder.timeline_module.StaticModuleDisplayTypeBuilder
  
   import com.twitter.product_mixer.component_library.decorator.urt.builder.timeline_module.TimelineModuleBuilder
  
   import com.twitter.product_mixer.component_library.filter.FeatureFilter
  
  +import com.twitter.product_mixer.component_library.filter.PredicateFeatureFilter
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.functional_component.candidate_source.CandidateSource
  
   import com.twitter.product_mixer.core.functional_component.decorator.CandidateDecorator
  
   import com.twitter.product_mixer.core.functional_component.feature_hydrator.BaseCandidateFeatureHydrator
  
   import com.twitter.product_mixer.core.functional_component.filter.Filter
  
   import com.twitter.product_mixer.core.functional_component.gate.Gate
  
  -import com.twitter.product_mixer.core.functional_component.scorer.Scorer
  
   import com.twitter.product_mixer.core.functional_component.transformer.CandidateFeatureTransformer
  
   import com.twitter.product_mixer.core.functional_component.transformer.CandidatePipelineQueryTransformer
  
   import com.twitter.product_mixer.core.functional_component.transformer.CandidatePipelineResultsTransformer
  
   @Singleton
  
   class ForYouScoredTweetsCandidatePipelineConfig @Inject() (
  
     scoredTweetsProductCandidateSource: ScoredTweetsProductCandidateSource,
  
  -  tweetypieFeatureHydrator: TweetypieFeatureHydrator,
  
  -  namesFeatureHydrator: NamesFeatureHydrator,
  
  -  sgsValidSocialContextFeatureHydrator: SGSValidSocialContextFeatureHydrator,
  
  -  perspectiveFilteredSocialContextFeatureHydrator: PerspectiveFilteredSocialContextFeatureHydrator,
  
     focalTweetFeatureHydrator: FocalTweetFeatureHydrator,
  
  +  namesFeatureHydrator: NamesFeatureHydrator,
  
  +  tweetypieFeatureHydrator: TweetypieFeatureHydrator,
  
  +  invalidSubscriptionTweetFilter: InvalidSubscriptionTweetFilter,
  
     homeFeedbackActionInfoBuilder: HomeFeedbackActionInfoBuilder,
  
     homeTweetSocialContextBuilder: HomeTweetSocialContextBuilder)
  
       extends CandidatePipelineConfig[
  
   
  
     override val preFilterFeatureHydrationPhase1: Seq[
  
       BaseCandidateFeatureHydrator[ForYouQuery, TweetCandidate, _]
  
  -  ] = Seq(
  
  -    namesFeatureHydrator,
  
  -    tweetypieFeatureHydrator,
  
  -    sgsValidSocialContextFeatureHydrator,
  
  -    perspectiveFilteredSocialContextFeatureHydrator,
  
  -  )
  
  +  ] = Seq(InNetworkFeatureHydrator, namesFeatureHydrator, tweetypieFeatureHydrator)
  
   
  
     override val filters: Seq[Filter[ForYouQuery, TweetCandidate]] = Seq(
  
       FeatureFilter.fromFeature(FilterIdentifier(TweetypieHydratedFilterId), IsHydratedFeature),
  
           !features.getOrElse(IsNsfwFeature, false)
  
         }
  
       ),
  
  -    FeedbackFatigueFilter,
  
       SocialContextFilter,
  
  +    invalidSubscriptionTweetFilter,
  
       InvalidConversationModuleFilter
  
     )
  
   
  
       BaseCandidateFeatureHydrator[ForYouQuery, TweetCandidate, _]
  
     ] = Seq(focalTweetFeatureHydrator)
  
   
  
  -  override val scorers: Seq[Scorer[ForYouQuery, TweetCandidate]] = Seq(FeedbackFatigueScorer)
  
  -
  
     override val decorator: Option[CandidateDecorator[ForYouQuery, TweetCandidate]] = {
  
       val clientEventInfoBuilder = HomeClientEventInfoBuilder()
  
   
  
