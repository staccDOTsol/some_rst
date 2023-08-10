a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouScoredTweetsMixerPipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouScoredTweetsMixerPipelineConfig.scala
==================================================

Change Range: -266,7 +325,7

Content:

::

index 29cb63678..2137c5267 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouScoredTweetsMixerPipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouScoredTweetsMixerPipelineConfig.scala
  
   import com.twitter.home_mixer.model.ClearCacheIncludeInstruction
  
   import com.twitter.home_mixer.model.HomeFeatures.InNetworkFeature
  
   import com.twitter.home_mixer.param.HomeGlobalParams.MaxNumberReplaceInstructionsParam
  
  +import com.twitter.home_mixer.param.HomeMixerFlagName.ScribeClientEventsFlag
  
   import com.twitter.home_mixer.product.following.model.HomeMixerExternalStrings
  
  +import com.twitter.home_mixer.product.for_you.feature_hydrator.TimelineServiceTweetsQueryFeatureHydrator
  
   import com.twitter.home_mixer.product.for_you.model.ForYouQuery
  
   import com.twitter.home_mixer.product.for_you.param.ForYouParam.ClearCacheOnPtr
  
   import com.twitter.home_mixer.product.for_you.param.ForYouParam.EnableFlipInjectionModuleCandidatePipelineParam
  
   import com.twitter.home_mixer.product.for_you.param.ForYouParam.FlipInlineInjectionModulePosition
  
   import com.twitter.home_mixer.product.for_you.param.ForYouParam.ServerMaxResultsParam
  
  +import com.twitter.home_mixer.product.for_you.param.ForYouParam.TweetPreviewsPositionParam
  
   import com.twitter.home_mixer.product.for_you.param.ForYouParam.WhoToFollowPositionParam
  
  +import com.twitter.home_mixer.product.for_you.param.ForYouParam.WhoToSubscribePositionParam
  
  +import com.twitter.home_mixer.product.for_you.side_effect.ServedCandidateFeatureKeysKafkaSideEffectBuilder
  
  +import com.twitter.home_mixer.product.for_you.side_effect.ServedCandidateKeysKafkaSideEffectBuilder
  
  +import com.twitter.home_mixer.product.for_you.side_effect.ServedStatsSideEffect
  
   import com.twitter.home_mixer.util.CandidatesUtil
  
  +import com.twitter.inject.annotations.Flag
  
   import com.twitter.logpipeline.client.common.EventPublisher
  
   import com.twitter.product_mixer.component_library.feature_hydrator.query.async.AsyncQueryFeatureHydrator
  
  +import com.twitter.product_mixer.component_library.feature_hydrator.query.social_graph.PreviewCreatorsQueryFeatureHydrator
  
  +import com.twitter.product_mixer.component_library.feature_hydrator.query.social_graph.SGSFollowedUsersQueryFeatureHydrator
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.component_library.pipeline.candidate.flexible_injection_pipeline.FlipPromptCandidatePipelineConfigBuilder
  
   import com.twitter.product_mixer.component_library.pipeline.candidate.who_to_follow_module.WhoToFollowCandidatePipelineConfig
  
  +import com.twitter.product_mixer.component_library.pipeline.candidate.who_to_subscribe_module.WhoToSubscribeCandidatePipelineConfig
  
   import com.twitter.product_mixer.component_library.premarshaller.urt.UrtDomainMarshaller
  
   import com.twitter.product_mixer.component_library.premarshaller.urt.builder.ClearCacheInstructionBuilder
  
   import com.twitter.product_mixer.component_library.premarshaller.urt.builder.OrderedBottomCursorBuilder
  
   
  
   @Singleton
  
   class ForYouScoredTweetsMixerPipelineConfig @Inject() (
  
  -  forYouScoredTweetsCandidatePipelineConfig: ForYouScoredTweetsCandidatePipelineConfig,
  
  +  forYouAdsDependentCandidatePipelineBuilder: ForYouAdsDependentCandidatePipelineBuilder,
  
     forYouConversationServiceCandidatePipelineConfig: ForYouConversationServiceCandidatePipelineConfig,
  
  -  forYouAdsCandidatePipelineBuilder: ForYouAdsCandidatePipelineBuilder,
  
  +  forYouPushToHomeTweetCandidatePipelineConfig: ForYouPushToHomeTweetCandidatePipelineConfig,
  
  +  forYouScoredTweetsCandidatePipelineConfig: ForYouScoredTweetsCandidatePipelineConfig,
  
     forYouWhoToFollowCandidatePipelineConfigBuilder: ForYouWhoToFollowCandidatePipelineConfigBuilder,
  
  +  forYouWhoToSubscribeCandidatePipelineConfigBuilder: ForYouWhoToSubscribeCandidatePipelineConfigBuilder,
  
     flipPromptCandidatePipelineConfigBuilder: FlipPromptCandidatePipelineConfigBuilder,
  
     editedTweetsCandidatePipelineConfig: EditedTweetsCandidatePipelineConfig,
  
     newTweetsPillCandidatePipelineConfig: NewTweetsPillCandidatePipelineConfig[ForYouQuery],
  
  +  forYouTweetPreviewsCandidatePipelineConfig: ForYouTweetPreviewsCandidatePipelineConfig,
  
     dismissInfoQueryFeatureHydrator: DismissInfoQueryFeatureHydrator,
  
     gizmoduckUserQueryFeatureHydrator: GizmoduckUserQueryFeatureHydrator,
  
     persistenceStoreQueryFeatureHydrator: PersistenceStoreQueryFeatureHydrator,
  
     requestQueryFeatureHydrator: RequestQueryFeatureHydrator[ForYouQuery],
  
  -  feedbackHistoryQueryFeatureHydrator: FeedbackHistoryQueryFeatureHydrator,
  
     timelineServiceTweetsQueryFeatureHydrator: TimelineServiceTweetsQueryFeatureHydrator,
  
  +  previewCreatorsQueryFeatureHydrator: PreviewCreatorsQueryFeatureHydrator,
  
  +  sgsFollowedUsersQueryFeatureHydrator: SGSFollowedUsersQueryFeatureHydrator,
  
     adsInjector: AdsInjector,
  
     servedCandidateKeysKafkaSideEffectBuilder: ServedCandidateKeysKafkaSideEffectBuilder,
  
     servedCandidateFeatureKeysKafkaSideEffectBuilder: ServedCandidateFeatureKeysKafkaSideEffectBuilder,
  
     updateTimelinesPersistenceStoreSideEffect: UpdateTimelinesPersistenceStoreSideEffect,
  
     truncateTimelinesPersistenceStoreSideEffect: TruncateTimelinesPersistenceStoreSideEffect,
  
  -  homeScribeServedEntriesSideEffect: HomeScribeServedEntriesSideEffect,
  
  +  homeScribeServedCandidatesSideEffect: HomeScribeServedCandidatesSideEffect,
  
     servedStatsSideEffect: ServedStatsSideEffect,
  
     clientEventsScribeEventPublisher: EventPublisher[ca.LogEvent],
  
     externalStrings: HomeMixerExternalStrings,
  
     @ProductScoped stringCenterProvider: Provider[StringCenter],
  
  -  urtTransportMarshaller: UrtTransportMarshaller)
  
  +  urtTransportMarshaller: UrtTransportMarshaller,
  
  +  @Flag(ScribeClientEventsFlag) enableScribeClientEvents: Boolean)
  
       extends MixerPipelineConfig[ForYouQuery, Timeline, urt.TimelineResponse] {
  
   
  
     override val identifier: MixerPipelineIdentifier = MixerPipelineIdentifier("ForYouScoredTweets")
  
   
  
     private val MaxConsecutiveOutOfNetworkCandidates = 2
  
   
  
  +  private val PushToHomeTweetPosition = 0
  
  +
  
     private val dependentCandidatesStep = MixerPipelineConfig.dependentCandidatePipelinesStep
  
   
  
     override val fetchQueryFeatures: Seq[QueryFeatureHydrator[ForYouQuery]] = Seq(
  
       requestQueryFeatureHydrator,
  
       persistenceStoreQueryFeatureHydrator,
  
       timelineServiceTweetsQueryFeatureHydrator,
  
  -    feedbackHistoryQueryFeatureHydrator,
  
  +    previewCreatorsQueryFeatureHydrator,
  
  +    sgsFollowedUsersQueryFeatureHydrator,
  
       AsyncQueryFeatureHydrator(dependentCandidatesStep, dismissInfoQueryFeatureHydrator),
  
       AsyncQueryFeatureHydrator(dependentCandidatesStep, gizmoduckUserQueryFeatureHydrator),
  
     )
  
   
  
  -  private val forYouAdsCandidatePipelineConfig = forYouAdsCandidatePipelineBuilder.build()
  
  +  private val scoredTweetsCandidatePipelineScope =
  
  +    SpecificPipeline(forYouScoredTweetsCandidatePipelineConfig.identifier)
  
  +
  
  +  private val forYouAdsCandidatePipelineConfig = forYouAdsDependentCandidatePipelineBuilder
  
  +    .build(scoredTweetsCandidatePipelineScope)
  
   
  
     private val forYouWhoToFollowCandidatePipelineConfig =
  
       forYouWhoToFollowCandidatePipelineConfigBuilder.build()
  
   
  
  +  private val forYouWhoToSubscribeCandidatePipelineConfig =
  
  +    forYouWhoToSubscribeCandidatePipelineConfigBuilder.build()
  
  +
  
     private val flipPromptCandidatePipelineConfig =
  
       flipPromptCandidatePipelineConfigBuilder.build[ForYouQuery](
  
         supportedClientParam = Some(EnableFlipInjectionModuleCandidatePipelineParam)
  
   
  
     override val candidatePipelines: Seq[CandidatePipelineConfig[ForYouQuery, _, _, _]] = Seq(
  
       forYouScoredTweetsCandidatePipelineConfig,
  
  -    forYouAdsCandidatePipelineConfig,
  
  +    forYouPushToHomeTweetCandidatePipelineConfig,
  
       forYouWhoToFollowCandidatePipelineConfig,
  
  +    forYouWhoToSubscribeCandidatePipelineConfig,
  
  +    forYouTweetPreviewsCandidatePipelineConfig,
  
       flipPromptCandidatePipelineConfig
  
     )
  
   
  
     override val dependentCandidatePipelines: Seq[
  
       DependentCandidatePipelineConfig[ForYouQuery, _, _, _]
  
     ] = Seq(
  
  +    forYouAdsCandidatePipelineConfig,
  
       forYouConversationServiceCandidatePipelineConfig,
  
       editedTweetsCandidatePipelineConfig,
  
       newTweetsPillCandidatePipelineConfig
  
       forYouScoredTweetsCandidatePipelineConfig.identifier -> FailOpenPolicy.Always,
  
       forYouAdsCandidatePipelineConfig.identifier -> FailOpenPolicy.Always,
  
       forYouWhoToFollowCandidatePipelineConfig.identifier -> FailOpenPolicy.Always,
  
  +    forYouWhoToSubscribeCandidatePipelineConfig.identifier -> FailOpenPolicy.Always,
  
  +    forYouTweetPreviewsCandidatePipelineConfig.identifier -> FailOpenPolicy.Always,
  
       flipPromptCandidatePipelineConfig.identifier -> FailOpenPolicy.Always,
  
       editedTweetsCandidatePipelineConfig.identifier -> FailOpenPolicy.Always,
  
       newTweetsPillCandidatePipelineConfig.identifier -> FailOpenPolicy.Always,
  
         candidatePipeline = editedTweetsCandidatePipelineConfig.identifier,
  
         maxSelectionsParam = MaxNumberReplaceInstructionsParam
  
       ),
  
  -    DropModuleTooFewModuleItemResults(
  
  -      candidatePipeline = forYouWhoToFollowCandidatePipelineConfig.identifier,
  
  -      minModuleItemsParam = StaticParam(WhoToFollowCandidatePipelineConfig.MinCandidatesSize)
  
  -    ),
  
       DropMaxModuleItemCandidates(
  
         candidatePipeline = forYouWhoToFollowCandidatePipelineConfig.identifier,
  
         maxModuleItemsParam = StaticParam(WhoToFollowCandidatePipelineConfig.MaxCandidatesSize)
  
       ),
  
  +    DropMaxModuleItemCandidates(
  
  +      candidatePipeline = forYouWhoToSubscribeCandidatePipelineConfig.identifier,
  
  +      maxModuleItemsParam = StaticParam(WhoToSubscribeCandidatePipelineConfig.MaxCandidatesSize)
  
  +    ),
  
       // The Conversation Service pipeline will only run if the Scored Tweets pipeline returned nothing
  
  -    InsertAppendResults(candidatePipeline =
  
  -      forYouConversationServiceCandidatePipelineConfig.identifier),
  
  -    InsertAppendResults(candidatePipeline = forYouScoredTweetsCandidatePipelineConfig.identifier),
  
  +    InsertAppendResults(
  
  +      candidatePipeline = forYouConversationServiceCandidatePipelineConfig.identifier
  
  +    ),
  
  +    InsertAppendResults(
  
  +      candidatePipeline = forYouScoredTweetsCandidatePipelineConfig.identifier
  
  +    ),
  
  +    InsertFixedPositionResults(
  
  +      candidatePipeline = forYouTweetPreviewsCandidatePipelineConfig.identifier,
  
  +      positionParam = TweetPreviewsPositionParam
  
  +    ),
  
       InsertFixedPositionResults(
  
         candidatePipeline = forYouWhoToFollowCandidatePipelineConfig.identifier,
  
         positionParam = WhoToFollowPositionParam
  
       ),
  
  +    InsertFixedPositionResults(
  
  +      candidatePipeline = forYouWhoToSubscribeCandidatePipelineConfig.identifier,
  
  +      positionParam = WhoToSubscribePositionParam
  
  +    ),
  
       InsertFixedPositionResults(
  
         candidatePipeline = flipPromptCandidatePipelineConfig.identifier,
  
         positionParam = FlipInlineInjectionModulePosition
  
       ),
  
  +    // Insert Push To Home Tweet at top of Timeline
  
  +    InsertFixedPositionResults(
  
  +      candidatePipeline = forYouPushToHomeTweetCandidatePipelineConfig.identifier,
  
  +      positionParam = StaticParam(PushToHomeTweetPosition)
  
  +    ),
  
       InsertAdResults(
  
         surfaceAreaName = AdsInjectionSurfaceAreas.HomeTimeline,
  
         adsInjector = adsInjector.forSurfaceArea(AdsInjectionSurfaceAreas.HomeTimeline),
  
         adsCandidatePipeline = forYouAdsCandidatePipelineConfig.identifier
  
       ),
  
       // This selector must come after the tweets are inserted into the results
  
  +    DropModuleTooFewModuleItemResults(
  
  +      candidatePipeline = forYouWhoToFollowCandidatePipelineConfig.identifier,
  
  +      minModuleItemsParam = StaticParam(WhoToFollowCandidatePipelineConfig.MinCandidatesSize)
  
  +    ),
  
  +    DropModuleTooFewModuleItemResults(
  
  +      candidatePipeline = forYouWhoToSubscribeCandidatePipelineConfig.identifier,
  
  +      minModuleItemsParam = StaticParam(WhoToSubscribeCandidatePipelineConfig.MinCandidatesSize)
  
  +    ),
  
       UpdateNewTweetsPillDecoration(
  
         pipelineScope = SpecificPipelines(
  
           forYouConversationServiceCandidatePipelineConfig.identifier,
  
         Set(forYouScoredTweetsCandidatePipelineConfig.identifier))
  
   
  
     private val homeScribeClientEventSideEffect = HomeScribeClientEventSideEffect(
  
  +    enableScribeClientEvents = enableScribeClientEvents,
  
       logPipelinePublisher = clientEventsScribeEventPublisher,
  
       injectedTweetsCandidatePipelineIdentifiers = Seq(
  
         forYouScoredTweetsCandidatePipelineConfig.identifier,
  
         forYouConversationServiceCandidatePipelineConfig.identifier
  
       ),
  
  -    adsCandidatePipelineIdentifier = forYouAdsCandidatePipelineConfig.identifier,
  
  -    whoToFollowCandidatePipelineIdentifier =
  
  -      Some(forYouWhoToFollowCandidatePipelineConfig.identifier),
  
  +    adsCandidatePipelineIdentifier = Some(forYouAdsCandidatePipelineConfig.identifier),
  
  +    whoToFollowCandidatePipelineIdentifier = Some(
  
  +      forYouWhoToFollowCandidatePipelineConfig.identifier
  
  +    ),
  
  +    whoToSubscribeCandidatePipelineIdentifier =
  
  +      Some(forYouWhoToSubscribeCandidatePipelineConfig.identifier)
  
     )
  
   
  
     override val resultSideEffects: Seq[PipelineResultSideEffect[ForYouQuery, Timeline]] = Seq(
  
       updateTimelinesPersistenceStoreSideEffect,
  
       truncateTimelinesPersistenceStoreSideEffect,
  
       homeScribeClientEventSideEffect,
  
  -    homeScribeServedEntriesSideEffect,
  
  +    homeScribeServedCandidatesSideEffect,
  
       servedStatsSideEffect
  
     )
  
   
  
