a/home-mixer/server/src/main/scala/com/twitter/home_mixer/candidate_pipeline/ConversationServiceCandidatePipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/candidate_pipeline/ConversationServiceCandidatePipelineConfig.scala
==================================================

Change Range: -93,6 +101,7

Content:

::

index 8a255b7bb..d26843193 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/candidate_pipeline/ConversationServiceCandidatePipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/candidate_pipeline/ConversationServiceCandidatePipelineConfig.scala
  
   package com.twitter.home_mixer.candidate_pipeline
  
   
  
  +import com.twitter.home_mixer.functional_component.feature_hydrator.InNetworkFeatureHydrator
  
   import com.twitter.home_mixer.functional_component.feature_hydrator.NamesFeatureHydrator
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.SocialGraphServiceFeatureHydrator
  
   import com.twitter.home_mixer.functional_component.feature_hydrator.TweetypieFeatureHydrator
  
   import com.twitter.home_mixer.functional_component.filter.InvalidConversationModuleFilter
  
  -import com.twitter.home_mixer.functional_component.filter.PredicateFeatureFilter
  
  +import com.twitter.home_mixer.functional_component.filter.InvalidSubscriptionTweetFilter
  
   import com.twitter.home_mixer.functional_component.filter.RetweetDeduplicationFilter
  
  +import com.twitter.home_mixer.model.HomeFeatures.AuthorIdFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.InReplyToTweetIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.IsHydratedFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.QuotedTweetDroppedFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.SourceTweetIdFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.SourceUserIdFeature
  
   import com.twitter.home_mixer.service.HomeMixerAlertConfig
  
   import com.twitter.product_mixer.component_library.candidate_source.tweetconvosvc.ConversationServiceCandidateSource
  
   import com.twitter.product_mixer.component_library.candidate_source.tweetconvosvc.ConversationServiceCandidateSourceRequest
  
   import com.twitter.product_mixer.component_library.candidate_source.tweetconvosvc.TweetWithConversationMetadata
  
   import com.twitter.product_mixer.component_library.filter.FeatureFilter
  
  +import com.twitter.product_mixer.component_library.filter.PredicateFeatureFilter
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.functional_component.candidate_source.BaseCandidateSource
  
   import com.twitter.product_mixer.core.functional_component.decorator.CandidateDecorator
  
   class ConversationServiceCandidatePipelineConfig[Query <: PipelineQuery](
  
     conversationServiceCandidateSource: ConversationServiceCandidateSource,
  
     tweetypieFeatureHydrator: TweetypieFeatureHydrator,
  
  -  socialGraphServiceFeatureHydrator: SocialGraphServiceFeatureHydrator,
  
     namesFeatureHydrator: NamesFeatureHydrator,
  
  +  invalidSubscriptionTweetFilter: InvalidSubscriptionTweetFilter,
  
     override val gates: Seq[BaseGate[Query]],
  
     override val decorator: Option[CandidateDecorator[Query, TweetCandidate]])
  
       extends DependentCandidatePipelineConfig[
  
       val tweetsWithConversationMetadata = candidates.map { candidate =>
  
         TweetWithConversationMetadata(
  
           tweetId = candidate.candidateIdLong,
  
  -        userId = None,
  
  -        sourceTweetId = None,
  
  -        sourceUserId = None,
  
  -        inReplyToTweetId = None,
  
  +        userId = candidate.features.getOrElse(AuthorIdFeature, None),
  
  +        sourceTweetId = candidate.features.getOrElse(SourceTweetIdFeature, None),
  
  +        sourceUserId = candidate.features.getOrElse(SourceUserIdFeature, None),
  
  +        inReplyToTweetId = candidate.features.getOrElse(InReplyToTweetIdFeature, None),
  
           conversationId = None,
  
           ancestors = Seq.empty
  
         )
  
   
  
     override val preFilterFeatureHydrationPhase1: Seq[
  
       BaseCandidateFeatureHydrator[Query, TweetCandidate, _]
  
  -  ] = Seq(tweetypieFeatureHydrator, socialGraphServiceFeatureHydrator)
  
  +  ] = Seq(
  
  +    tweetypieFeatureHydrator,
  
  +    InNetworkFeatureHydrator,
  
  +  )
  
   
  
     override def filters: Seq[Filter[Query, TweetCandidate]] = Seq(
  
       RetweetDeduplicationFilter,
  
         FilterIdentifier(QuotedTweetDroppedFilterId),
  
         shouldKeepCandidate = { features => !features.getOrElse(QuotedTweetDroppedFeature, false) }
  
       ),
  
  +    invalidSubscriptionTweetFilter,
  
       InvalidConversationModuleFilter
  
     )
  
   
  
