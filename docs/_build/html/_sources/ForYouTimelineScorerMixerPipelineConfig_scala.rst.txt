a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouTimelineScorerMixerPipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouTimelineScorerMixerPipelineConfig.scala
==================================================

Change Range: -266,27 +329,31

Content:

::

index b43c6ed67..441e5796d 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouTimelineScorerMixerPipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouTimelineScorerMixerPipelineConfig.scala
  
   import com.twitter.home_mixer.candidate_pipeline.EditedTweetsCandidatePipelineConfig
  
   import com.twitter.home_mixer.candidate_pipeline.NewTweetsPillCandidatePipelineConfig
  
   import com.twitter.home_mixer.functional_component.decorator.urt.builder.AddEntriesWithReplaceAndShowAlertAndCoverInstructionBuilder
  
  +import com.twitter.home_mixer.functional_component.feature_hydrator.FeedbackHistoryQueryFeatureHydrator
  
   import com.twitter.home_mixer.functional_component.feature_hydrator._
  
   import com.twitter.home_mixer.functional_component.selector.DebunchCandidates
  
   import com.twitter.home_mixer.functional_component.selector.UpdateConversationModuleId
  
   import com.twitter.home_mixer.functional_component.side_effect._
  
   import com.twitter.home_mixer.model.ClearCacheIncludeInstruction
  
   import com.twitter.home_mixer.model.HomeFeatures.InNetworkFeature
  
  +import com.twitter.home_mixer.param.HomeGlobalParams.EnableImpressionBloomFilter
  
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
  
   import com.twitter.product_mixer.component_library.feature_hydrator.query.impressed_tweets.ImpressedTweetsQueryFeatureHydrator
  
  +import com.twitter.product_mixer.component_library.feature_hydrator.query.param_gated.AsyncParamGatedQueryFeatureHydrator
  
  +import com.twitter.product_mixer.component_library.feature_hydrator.query.social_graph.PreviewCreatorsQueryFeatureHydrator
  
  +import com.twitter.product_mixer.component_library.feature_hydrator.query.social_graph.SGSFollowedUsersQueryFeatureHydrator
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.component_library.pipeline.candidate.flexible_injection_pipeline.FlipPromptCandidatePipelineConfigBuilder
  
  -import com.twitter.product_mixer.component_library.pipeline.candidate.who_to_follow_module.WhoToFollowCandidatePipelineConfig
  
  +import com.twitter.product_mixer.component_library.pipeline.candidate.who_to_follow_module.WhoToFollowArmCandidatePipelineConfig
  
  +import com.twitter.product_mixer.component_library.pipeline.candidate.who_to_subscribe_module.WhoToSubscribeCandidatePipelineConfig
  
   import com.twitter.product_mixer.component_library.premarshaller.urt.UrtDomainMarshaller
  
   import com.twitter.product_mixer.component_library.premarshaller.urt.builder.ClearCacheInstructionBuilder
  
   import com.twitter.product_mixer.component_library.premarshaller.urt.builder.OrderedBottomCursorBuilder
  
   @Singleton
  
   class ForYouTimelineScorerMixerPipelineConfig @Inject() (
  
     forYouTimelineScorerCandidatePipelineConfig: ForYouTimelineScorerCandidatePipelineConfig,
  
  +  forYouPushToHomeTweetCandidatePipelineConfig: ForYouPushToHomeTweetCandidatePipelineConfig,
  
     forYouConversationServiceCandidatePipelineConfig: ForYouConversationServiceCandidatePipelineConfig,
  
  -  forYouAdsCandidatePipelineBuilder: ForYouAdsCandidatePipelineBuilder,
  
  +  forYouAdsDependentCandidatePipelineBuilder: ForYouAdsDependentCandidatePipelineBuilder,
  
     forYouWhoToFollowCandidatePipelineConfigBuilder: ForYouWhoToFollowCandidatePipelineConfigBuilder,
  
  +  forYouWhoToSubscribeCandidatePipelineConfigBuilder: ForYouWhoToSubscribeCandidatePipelineConfigBuilder,
  
     flipPromptCandidatePipelineConfigBuilder: FlipPromptCandidatePipelineConfigBuilder,
  
     editedTweetsCandidatePipelineConfig: EditedTweetsCandidatePipelineConfig,
  
     newTweetsPillCandidatePipelineConfig: NewTweetsPillCandidatePipelineConfig[ForYouQuery],
  
  +  forYouTweetPreviewsCandidatePipelineConfig: ForYouTweetPreviewsCandidatePipelineConfig,
  
     dismissInfoQueryFeatureHydrator: DismissInfoQueryFeatureHydrator,
  
     gizmoduckUserQueryFeatureHydrator: GizmoduckUserQueryFeatureHydrator,
  
  +  impressionBloomFilterQueryFeatureHydrator: ImpressionBloomFilterQueryFeatureHydrator[ForYouQuery],
  
     manhattanTweetImpressionsQueryFeatureHydrator: TweetImpressionsQueryFeatureHydrator[ForYouQuery],
  
     memcacheTweetImpressionsQueryFeatureHydrator: ImpressedTweetsQueryFeatureHydrator,
  
     persistenceStoreQueryFeatureHydrator: PersistenceStoreQueryFeatureHydrator,
  
     timelineServiceTweetsQueryFeatureHydrator: TimelineServiceTweetsQueryFeatureHydrator,
  
     lastNonPollingTimeQueryFeatureHydrator: LastNonPollingTimeQueryFeatureHydrator,
  
     feedbackHistoryQueryFeatureHydrator: FeedbackHistoryQueryFeatureHydrator,
  
  +  previewCreatorsQueryFeatureHydrator: PreviewCreatorsQueryFeatureHydrator,
  
  +  sgsFollowedUsersQueryFeatureHydrator: SGSFollowedUsersQueryFeatureHydrator,
  
     adsInjector: AdsInjector,
  
     servedCandidateKeysKafkaSideEffectBuilder: ServedCandidateKeysKafkaSideEffectBuilder,
  
     servedCandidateFeatureKeysKafkaSideEffectBuilder: ServedCandidateFeatureKeysKafkaSideEffectBuilder,
  
     updateLastNonPollingTimeSideEffect: UpdateLastNonPollingTimeSideEffect[ForYouQuery, Timeline],
  
     publishClientSentImpressionsEventBusSideEffect: PublishClientSentImpressionsEventBusSideEffect,
  
     publishClientSentImpressionsManhattanSideEffect: PublishClientSentImpressionsManhattanSideEffect,
  
  +  publishImpressionBloomFilterSideEffect: PublishImpressionBloomFilterSideEffect,
  
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
  
   
  
     override val identifier: MixerPipelineIdentifier = MixerPipelineIdentifier("ForYouTimelineScorer")
  
   
  
     private val MaxConsecutiveOutOfNetworkCandidates = 2
  
   
  
  +  private val PushToHomeTweetPosition = 0
  
  +
  
     private val dependentCandidatesStep = MixerPipelineConfig.dependentCandidatePipelinesStep
  
     private val resultSelectorsStep = MixerPipelineConfig.resultSelectorsStep
  
   
  
       persistenceStoreQueryFeatureHydrator,
  
       timelineServiceTweetsQueryFeatureHydrator,
  
       feedbackHistoryQueryFeatureHydrator,
  
  +    previewCreatorsQueryFeatureHydrator,
  
  +    sgsFollowedUsersQueryFeatureHydrator,
  
       AsyncQueryFeatureHydrator(dependentCandidatesStep, dismissInfoQueryFeatureHydrator),
  
       AsyncQueryFeatureHydrator(dependentCandidatesStep, gizmoduckUserQueryFeatureHydrator),
  
       AsyncQueryFeatureHydrator(dependentCandidatesStep, lastNonPollingTimeQueryFeatureHydrator),
  
  +    AsyncParamGatedQueryFeatureHydrator(
  
  +      EnableImpressionBloomFilter,
  
  +      resultSelectorsStep,
  
  +      impressionBloomFilterQueryFeatureHydrator),
  
       AsyncQueryFeatureHydrator(resultSelectorsStep, manhattanTweetImpressionsQueryFeatureHydrator),
  
       AsyncQueryFeatureHydrator(resultSelectorsStep, memcacheTweetImpressionsQueryFeatureHydrator)
  
     )
  
   
  
  -  private val forYouAdsCandidatePipelineConfig = forYouAdsCandidatePipelineBuilder.build()
  
  +  private val timelineScorerCandidatePipelineScope =
  
  +    SpecificPipeline(forYouTimelineScorerCandidatePipelineConfig.identifier)
  
  +
  
  +  private val forYouAdsCandidatePipelineConfig = forYouAdsDependentCandidatePipelineBuilder
  
  +    .build(timelineScorerCandidatePipelineScope)
  
   
  
     private val forYouWhoToFollowCandidatePipelineConfig =
  
       forYouWhoToFollowCandidatePipelineConfigBuilder.build()
  
   
  
  +  private val forYouWhoToSubscribeCandidatePipelineConfig =
  
  +    forYouWhoToSubscribeCandidatePipelineConfigBuilder.build()
  
  +
  
     private val flipPromptCandidatePipelineConfig =
  
       flipPromptCandidatePipelineConfigBuilder.build[ForYouQuery](
  
         supportedClientParam = Some(EnableFlipInjectionModuleCandidatePipelineParam)
  
   
  
     override val candidatePipelines: Seq[CandidatePipelineConfig[ForYouQuery, _, _, _]] = Seq(
  
       forYouTimelineScorerCandidatePipelineConfig,
  
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
  
       forYouTimelineScorerCandidatePipelineConfig.identifier -> FailOpenPolicy.Always,
  
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
  
  +    DropMaxModuleItemCandidates(
  
         candidatePipeline = forYouWhoToFollowCandidatePipelineConfig.identifier,
  
  -      minModuleItemsParam = StaticParam(WhoToFollowCandidatePipelineConfig.MinCandidatesSize)
  
  +      maxModuleItemsParam = StaticParam(WhoToFollowArmCandidatePipelineConfig.MaxCandidatesSize)
  
  +    ),
  
  +    DropModuleTooFewModuleItemResults(
  
  +      candidatePipeline = forYouWhoToSubscribeCandidatePipelineConfig.identifier,
  
  +      minModuleItemsParam = StaticParam(WhoToSubscribeCandidatePipelineConfig.MinCandidatesSize)
  
       ),
  
       DropMaxModuleItemCandidates(
  
  -      candidatePipeline = forYouWhoToFollowCandidatePipelineConfig.identifier,
  
  -      maxModuleItemsParam = StaticParam(WhoToFollowCandidatePipelineConfig.MaxCandidatesSize)
  
  +      candidatePipeline = forYouWhoToSubscribeCandidatePipelineConfig.identifier,
  
  +      maxModuleItemsParam = StaticParam(WhoToSubscribeCandidatePipelineConfig.MaxCandidatesSize)
  
       ),
  
       // Add Conversation Service tweets to results only if the scored pipeline doesn't return any
  
       SelectConditionally(
  
             forYouTimelineScorerCandidatePipelineConfig.identifier == candidate.source)
  
       ),
  
       InsertAppendResults(candidatePipeline = forYouTimelineScorerCandidatePipelineConfig.identifier),
  
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
  
  +      minModuleItemsParam = StaticParam(WhoToFollowArmCandidatePipelineConfig.MinCandidatesSize)
  
  +    ),
  
       UpdateNewTweetsPillDecoration(
  
         pipelineScope = SpecificPipelines(
  
           forYouConversationServiceCandidatePipelineConfig.identifier,
  
         Set(forYouTimelineScorerCandidatePipelineConfig.identifier))
  
   
  
     private val homeScribeClientEventSideEffect = HomeScribeClientEventSideEffect(
  
  +    enableScribeClientEvents = enableScribeClientEvents,
  
       logPipelinePublisher = clientEventsScribeEventPublisher,
  
       injectedTweetsCandidatePipelineIdentifiers = Seq(
  
         forYouTimelineScorerCandidatePipelineConfig.identifier,
  
         forYouConversationServiceCandidatePipelineConfig.identifier
  
       ),
  
  -    adsCandidatePipelineIdentifier = forYouAdsCandidatePipelineConfig.identifier,
  
  +    adsCandidatePipelineIdentifier = Some(forYouAdsCandidatePipelineConfig.identifier),
  
       whoToFollowCandidatePipelineIdentifier =
  
         Some(forYouWhoToFollowCandidatePipelineConfig.identifier),
  
  +    whoToSubscribeCandidatePipelineIdentifier =
  
  +      Some(forYouWhoToSubscribeCandidatePipelineConfig.identifier)
  
     )
  
   
  
     override val resultSideEffects: Seq[PipelineResultSideEffect[ForYouQuery, Timeline]] = Seq(
  
  -    servedCandidateKeysKafkaSideEffect,
  
  -    servedCandidateFeatureKeysKafkaSideEffect,
  
  -    updateLastNonPollingTimeSideEffect,
  
  +    homeScribeClientEventSideEffect,
  
  +    homeScribeServedCandidatesSideEffect,
  
       publishClientSentImpressionsEventBusSideEffect,
  
       publishClientSentImpressionsManhattanSideEffect,
  
  -    updateTimelinesPersistenceStoreSideEffect,
  
  +    publishImpressionBloomFilterSideEffect,
  
  +    servedCandidateKeysKafkaSideEffect,
  
  +    servedCandidateFeatureKeysKafkaSideEffect,
  
  +    servedStatsSideEffect,
  
       truncateTimelinesPersistenceStoreSideEffect,
  
  -    homeScribeClientEventSideEffect,
  
  -    homeScribeServedEntriesSideEffect,
  
  -    servedStatsSideEffect
  
  +    updateLastNonPollingTimeSideEffect,
  
  +    updateTimelinesPersistenceStoreSideEffect
  
     )
  
   
  
     override val domainMarshaller: DomainMarshaller[ForYouQuery, Timeline] = {
  
