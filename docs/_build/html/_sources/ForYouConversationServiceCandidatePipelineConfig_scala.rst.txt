a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouConversationServiceCandidatePipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouConversationServiceCandidatePipelineConfig.scala
==================================================

Change Range: -107,17 +107,20

Content:

::

index 3f69ad888..494d3a584 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouConversationServiceCandidatePipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouConversationServiceCandidatePipelineConfig.scala
  
   
  
   import com.twitter.home_mixer.candidate_pipeline.ConversationServiceResponseFeatureTransformer
  
   import com.twitter.home_mixer.functional_component.decorator.HomeConversationServiceCandidateDecorator
  
  -import com.twitter.home_mixer.functional_component.decorator.HomeFeedbackActionInfoBuilder
  
  +import com.twitter.home_mixer.functional_component.decorator.urt.builder.HomeFeedbackActionInfoBuilder
  
  +import com.twitter.home_mixer.functional_component.feature_hydrator.InNetworkFeatureHydrator
  
   import com.twitter.home_mixer.functional_component.feature_hydrator.NamesFeatureHydrator
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.SocialGraphServiceFeatureHydrator
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.TimelineServiceTweetsFeature
  
   import com.twitter.home_mixer.functional_component.feature_hydrator.TweetypieFeatureHydrator
  
   import com.twitter.home_mixer.functional_component.filter.InvalidConversationModuleFilter
  
  -import com.twitter.home_mixer.functional_component.filter.PredicateFeatureFilter
  
  -import com.twitter.home_mixer.functional_component.filter.PreviouslySeenTweetsFilter
  
  +import com.twitter.home_mixer.functional_component.filter.InvalidSubscriptionTweetFilter
  
   import com.twitter.home_mixer.functional_component.filter.PreviouslyServedTweetsFilter
  
   import com.twitter.home_mixer.functional_component.filter.RetweetDeduplicationFilter
  
  -import com.twitter.home_mixer.functional_component.gate.NonEmptySeqFeatureGate
  
   import com.twitter.home_mixer.model.HomeFeatures.IsHydratedFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.QuotedTweetDroppedFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.TimelineServiceTweetsFeature
  
   import com.twitter.home_mixer.product.for_you.model.ForYouQuery
  
   import com.twitter.home_mixer.service.HomeMixerAlertConfig
  
   import com.twitter.product_mixer.component_library.candidate_source.tweetconvosvc.ConversationServiceCandidateSource
  
   import com.twitter.product_mixer.component_library.candidate_source.tweetconvosvc.ConversationServiceCandidateSourceRequest
  
   import com.twitter.product_mixer.component_library.candidate_source.tweetconvosvc.TweetWithConversationMetadata
  
   import com.twitter.product_mixer.component_library.filter.FeatureFilter
  
  +import com.twitter.product_mixer.component_library.filter.PredicateFeatureFilter
  
   import com.twitter.product_mixer.component_library.gate.NoCandidatesGate
  
  +import com.twitter.product_mixer.component_library.gate.NonEmptySeqFeatureGate
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.functional_component.candidate_source.BaseCandidateSource
  
   import com.twitter.product_mixer.core.functional_component.common.SpecificPipelines
  
     forYouTimelineScorerCandidatePipelineConfig: ForYouTimelineScorerCandidatePipelineConfig,
  
     conversationServiceCandidateSource: ConversationServiceCandidateSource,
  
     tweetypieFeatureHydrator: TweetypieFeatureHydrator,
  
  -  socialGraphServiceFeatureHydrator: SocialGraphServiceFeatureHydrator,
  
     namesFeatureHydrator: NamesFeatureHydrator,
  
  +  invalidSubscriptionTweetFilter: InvalidSubscriptionTweetFilter,
  
     homeFeedbackActionInfoBuilder: HomeFeedbackActionInfoBuilder)
  
       extends DependentCandidatePipelineConfig[
  
         ForYouQuery,
  
   
  
     override val preFilterFeatureHydrationPhase1: Seq[
  
       BaseCandidateFeatureHydrator[ForYouQuery, TweetCandidate, _]
  
  -  ] = Seq(tweetypieFeatureHydrator, socialGraphServiceFeatureHydrator)
  
  +  ] = Seq(
  
  +    InNetworkFeatureHydrator,
  
  +    tweetypieFeatureHydrator
  
  +  )
  
   
  
     override def filters: Seq[Filter[ForYouQuery, TweetCandidate]] = Seq(
  
       PreviouslyServedTweetsFilter,
  
  -    PreviouslySeenTweetsFilter,
  
       RetweetDeduplicationFilter,
  
       FeatureFilter.fromFeature(FilterIdentifier("TweetypieHydrated"), IsHydratedFeature),
  
       PredicateFeatureFilter.fromPredicate(
  
         FilterIdentifier("QuotedTweetDropped"),
  
         shouldKeepCandidate = { features => !features.getOrElse(QuotedTweetDroppedFeature, false) }
  
       ),
  
  +    invalidSubscriptionTweetFilter,
  
       InvalidConversationModuleFilter
  
     )
  
   
  
