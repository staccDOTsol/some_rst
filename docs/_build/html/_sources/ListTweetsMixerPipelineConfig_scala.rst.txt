a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_tweets/ListTweetsMixerPipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_tweets/ListTweetsMixerPipelineConfig.scala
==================================================

Change Range: -101,14 +106,16

Content:

::

index f4e20ce7e..2d7dd3b31 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_tweets/ListTweetsMixerPipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_tweets/ListTweetsMixerPipelineConfig.scala
  
   import com.twitter.clientapp.{thriftscala => ca}
  
   import com.twitter.goldfinch.api.AdsInjectionSurfaceAreas
  
   import com.twitter.home_mixer.candidate_pipeline.ConversationServiceCandidatePipelineConfigBuilder
  
  -import com.twitter.home_mixer.functional_component.decorator.ListConversationServiceCandidateDecorator
  
   import com.twitter.home_mixer.functional_component.feature_hydrator.RequestQueryFeatureHydrator
  
   import com.twitter.home_mixer.functional_component.side_effect.HomeScribeClientEventSideEffect
  
   import com.twitter.home_mixer.model.GapIncludeInstruction
  
  +import com.twitter.home_mixer.param.HomeMixerFlagName.ScribeClientEventsFlag
  
  +import com.twitter.home_mixer.product.list_tweets.decorator.ListConversationServiceCandidateDecorator
  
   import com.twitter.home_mixer.product.list_tweets.model.ListTweetsQuery
  
   import com.twitter.home_mixer.util.CandidatesUtil
  
  +import com.twitter.inject.annotations.Flag
  
   import com.twitter.logpipeline.client.common.EventPublisher
  
  +import com.twitter.product_mixer.component_library.feature_hydrator.query.social_graph.SGSFollowedUsersQueryFeatureHydrator
  
   import com.twitter.product_mixer.component_library.gate.NonEmptyCandidatesGate
  
   import com.twitter.product_mixer.component_library.premarshaller.urt.UrtDomainMarshaller
  
   import com.twitter.product_mixer.component_library.premarshaller.urt.builder.AddEntriesWithReplaceAndShowAlertInstructionBuilder
  
     ],
  
     listTweetsAdsCandidatePipelineBuilder: ListTweetsAdsCandidatePipelineBuilder,
  
     requestQueryFeatureHydrator: RequestQueryFeatureHydrator[ListTweetsQuery],
  
  +  sgsFollowedUsersQueryFeatureHydrator: SGSFollowedUsersQueryFeatureHydrator,
  
     adsInjector: AdsInjector,
  
     clientEventsScribeEventPublisher: EventPublisher[ca.LogEvent],
  
  -  urtTransportMarshaller: UrtTransportMarshaller)
  
  +  urtTransportMarshaller: UrtTransportMarshaller,
  
  +  @Flag(ScribeClientEventsFlag) enableScribeClientEvents: Boolean)
  
       extends MixerPipelineConfig[ListTweetsQuery, Timeline, urt.TimelineResponse] {
  
   
  
     override val identifier: MixerPipelineIdentifier = MixerPipelineIdentifier("ListTweets")
  
     )
  
   
  
     override val fetchQueryFeatures: Seq[QueryFeatureHydrator[ListTweetsQuery]] = Seq(
  
  -    requestQueryFeatureHydrator
  
  +    requestQueryFeatureHydrator,
  
  +    sgsFollowedUsersQueryFeatureHydrator
  
     )
  
   
  
     private val homeScribeClientEventSideEffect = HomeScribeClientEventSideEffect(
  
  +    enableScribeClientEvents = enableScribeClientEvents,
  
       logPipelinePublisher = clientEventsScribeEventPublisher,
  
       injectedTweetsCandidatePipelineIdentifiers =
  
         Seq(conversationServiceCandidatePipelineConfig.identifier),
  
  -    adsCandidatePipelineIdentifier = listTweetsAdsCandidatePipelineConfig.identifier,
  
  +    adsCandidatePipelineIdentifier = Some(listTweetsAdsCandidatePipelineConfig.identifier),
  
     )
  
   
  
     override val resultSideEffects: Seq[PipelineResultSideEffect[ListTweetsQuery, Timeline]] =
  
