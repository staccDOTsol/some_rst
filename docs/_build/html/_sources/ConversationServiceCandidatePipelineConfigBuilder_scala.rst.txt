a/home-mixer/server/src/main/scala/com/twitter/home_mixer/candidate_pipeline/ConversationServiceCandidatePipelineConfigBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/candidate_pipeline/ConversationServiceCandidatePipelineConfigBuilder.scala
==================================================

Change Range: -25,8 +25,8

Content:

::

index 219069816..bb55f85e3 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/candidate_pipeline/ConversationServiceCandidatePipelineConfigBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/candidate_pipeline/ConversationServiceCandidatePipelineConfigBuilder.scala
  
   package com.twitter.home_mixer.candidate_pipeline
  
   
  
  -import com.twitter.product_mixer.component_library.candidate_source.tweetconvosvc.ConversationServiceCandidateSource
  
   import com.twitter.home_mixer.functional_component.feature_hydrator.NamesFeatureHydrator
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.SocialGraphServiceFeatureHydrator
  
   import com.twitter.home_mixer.functional_component.feature_hydrator.TweetypieFeatureHydrator
  
  +import com.twitter.home_mixer.functional_component.filter.InvalidSubscriptionTweetFilter
  
  +import com.twitter.product_mixer.component_library.candidate_source.tweetconvosvc.ConversationServiceCandidateSource
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.functional_component.decorator.CandidateDecorator
  
   import com.twitter.product_mixer.core.functional_component.gate.BaseGate
  
   class ConversationServiceCandidatePipelineConfigBuilder[Query <: PipelineQuery] @Inject() (
  
     conversationServiceCandidateSource: ConversationServiceCandidateSource,
  
     tweetypieFeatureHydrator: TweetypieFeatureHydrator,
  
  -  socialGraphServiceFeatureHydrator: SocialGraphServiceFeatureHydrator,
  
  +  invalidSubscriptionTweetFilter: InvalidSubscriptionTweetFilter,
  
     namesFeatureHydrator: NamesFeatureHydrator) {
  
   
  
     def build(
  
       new ConversationServiceCandidatePipelineConfig(
  
         conversationServiceCandidateSource,
  
         tweetypieFeatureHydrator,
  
  -      socialGraphServiceFeatureHydrator,
  
         namesFeatureHydrator,
  
  +      invalidSubscriptionTweetFilter,
  
         gates,
  
         decorator
  
       )
  
