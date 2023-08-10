a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingMixerPipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingMixerPipelineConfig.scala
==================================================

Change Range: -221,22 +240,24

Content:

::

index 23b4164a5..efda03595 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingMixerPipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingMixerPipelineConfig.scala
  
   import com.twitter.home_mixer.candidate_pipeline.EditedTweetsCandidatePipelineConfig
  
   import com.twitter.home_mixer.candidate_pipeline.NewTweetsPillCandidatePipelineConfig
  
   import com.twitter.home_mixer.functional_component.decorator.HomeConversationServiceCandidateDecorator
  
  -import com.twitter.home_mixer.functional_component.decorator.HomeFeedbackActionInfoBuilder
  
   import com.twitter.home_mixer.functional_component.decorator.urt.builder.AddEntriesWithReplaceAndShowAlertAndCoverInstructionBuilder
  
  +import com.twitter.home_mixer.functional_component.decorator.urt.builder.HomeFeedbackActionInfoBuilder
  
   import com.twitter.home_mixer.functional_component.feature_hydrator._
  
   import com.twitter.home_mixer.functional_component.selector.UpdateHomeClientEventDetails
  
   import com.twitter.home_mixer.functional_component.selector.UpdateNewTweetsPillDecoration
  
   import com.twitter.home_mixer.functional_component.side_effect._
  
   import com.twitter.home_mixer.model.GapIncludeInstruction
  
  +import com.twitter.home_mixer.param.HomeGlobalParams.EnableImpressionBloomFilter
  
   import com.twitter.home_mixer.param.HomeGlobalParams.MaxNumberReplaceInstructionsParam
  
  +import com.twitter.home_mixer.param.HomeMixerFlagName.ScribeClientEventsFlag
  
  +import com.twitter.product_mixer.component_library.feature_hydrator.query.social_graph.SGSFollowedUsersQueryFeatureHydrator
  
   import com.twitter.home_mixer.product.following.model.FollowingQuery
  
   import com.twitter.home_mixer.product.following.model.HomeMixerExternalStrings
  
   import com.twitter.home_mixer.product.following.param.FollowingParam.EnableFlipInjectionModuleCandidatePipelineParam
  
   import com.twitter.home_mixer.product.following.param.FollowingParam.ServerMaxResultsParam
  
   import com.twitter.home_mixer.product.following.param.FollowingParam.WhoToFollowPositionParam
  
   import com.twitter.home_mixer.util.CandidatesUtil
  
  +import com.twitter.inject.annotations.Flag
  
   import com.twitter.logpipeline.client.common.EventPublisher
  
   import com.twitter.product_mixer.component_library.feature_hydrator.query.async.AsyncQueryFeatureHydrator
  
  +import com.twitter.product_mixer.component_library.feature_hydrator.query.impressed_tweets.ImpressedTweetsQueryFeatureHydrator
  
  +import com.twitter.product_mixer.component_library.feature_hydrator.query.param_gated.AsyncParamGatedQueryFeatureHydrator
  
   import com.twitter.product_mixer.component_library.gate.NonEmptyCandidatesGate
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.component_library.pipeline.candidate.flexible_injection_pipeline.FlipPromptDependentCandidatePipelineConfigBuilder
  
     ],
  
     homeFeedbackActionInfoBuilder: HomeFeedbackActionInfoBuilder,
  
     followingAdsCandidatePipelineBuilder: FollowingAdsCandidatePipelineBuilder,
  
  -  followingWhoToFollowArmCandidatePipelineConfigBuilder: FollowingWhoToFollowArmCandidatePipelineConfigBuilder,
  
  +  followingWhoToFollowCandidatePipelineConfigBuilder: FollowingWhoToFollowCandidatePipelineConfigBuilder,
  
     flipPromptDependentCandidatePipelineConfigBuilder: FlipPromptDependentCandidatePipelineConfigBuilder,
  
     editedTweetsCandidatePipelineConfig: EditedTweetsCandidatePipelineConfig,
  
     newTweetsPillCandidatePipelineConfig: NewTweetsPillCandidatePipelineConfig[FollowingQuery],
  
     realGraphInNetworkSourceQueryHydrator: RealGraphInNetworkScoresQueryFeatureHydrator,
  
     requestQueryFeatureHydrator: RequestQueryFeatureHydrator[FollowingQuery],
  
     sgsFollowedUsersQueryFeatureHydrator: SGSFollowedUsersQueryFeatureHydrator,
  
  -  tweetImpressionsQueryFeatureHydrator: TweetImpressionsQueryFeatureHydrator[FollowingQuery],
  
  +  impressionBloomFilterQueryFeatureHydrator: ImpressionBloomFilterQueryFeatureHydrator[
  
  +    FollowingQuery
  
  +  ],
  
  +  manhattanTweetImpressionsQueryFeatureHydrator: TweetImpressionsQueryFeatureHydrator[
  
  +    FollowingQuery
  
  +  ],
  
  +  memcacheTweetImpressionsQueryFeatureHydrator: ImpressedTweetsQueryFeatureHydrator,
  
     lastNonPollingTimeQueryFeatureHydrator: LastNonPollingTimeQueryFeatureHydrator,
  
     adsInjector: AdsInjector,
  
     updateLastNonPollingTimeSideEffect: UpdateLastNonPollingTimeSideEffect[FollowingQuery, Timeline],
  
     publishClientSentImpressionsEventBusSideEffect: PublishClientSentImpressionsEventBusSideEffect,
  
     publishClientSentImpressionsManhattanSideEffect: PublishClientSentImpressionsManhattanSideEffect,
  
  +  publishImpressionBloomFilterSideEffect: PublishImpressionBloomFilterSideEffect,
  
     updateTimelinesPersistenceStoreSideEffect: UpdateTimelinesPersistenceStoreSideEffect,
  
     truncateTimelinesPersistenceStoreSideEffect: TruncateTimelinesPersistenceStoreSideEffect,
  
  -  homeTimelineServedEntriesSideEffect: HomeScribeServedEntriesSideEffect,
  
  +  homeTimelineServedCandidatesSideEffect: HomeScribeServedCandidatesSideEffect,
  
     clientEventsScribeEventPublisher: EventPublisher[ca.LogEvent],
  
     externalStrings: HomeMixerExternalStrings,
  
     @ProductScoped stringCenterProvider: Provider[StringCenter],
  
  -  urtTransportMarshaller: UrtTransportMarshaller)
  
  +  urtTransportMarshaller: UrtTransportMarshaller,
  
  +  @Flag(ScribeClientEventsFlag) enableScribeClientEvents: Boolean)
  
       extends MixerPipelineConfig[FollowingQuery, Timeline, urt.TimelineResponse] {
  
   
  
     override val identifier: MixerPipelineIdentifier = MixerPipelineIdentifier("Following")
  
       AsyncQueryFeatureHydrator(dependentCandidatesStep, gizmoduckUserQueryFeatureHydrator),
  
       AsyncQueryFeatureHydrator(dependentCandidatesStep, persistenceStoreQueryFeatureHydrator),
  
       AsyncQueryFeatureHydrator(dependentCandidatesStep, lastNonPollingTimeQueryFeatureHydrator),
  
  -    AsyncQueryFeatureHydrator(resultSelectorsStep, tweetImpressionsQueryFeatureHydrator),
  
  +    AsyncParamGatedQueryFeatureHydrator(
  
  +      EnableImpressionBloomFilter,
  
  +      resultSelectorsStep,
  
  +      impressionBloomFilterQueryFeatureHydrator),
  
  +    AsyncQueryFeatureHydrator(resultSelectorsStep, manhattanTweetImpressionsQueryFeatureHydrator),
  
  +    AsyncQueryFeatureHydrator(resultSelectorsStep, memcacheTweetImpressionsQueryFeatureHydrator)
  
     )
  
   
  
     private val earlybirdCandidatePipelineScope =
  
     private val followingAdsCandidatePipelineConfig =
  
       followingAdsCandidatePipelineBuilder.build(earlybirdCandidatePipelineScope)
  
   
  
  -  private val followingWhoToFollowArmCandidatePipelineConfig =
  
  -    followingWhoToFollowArmCandidatePipelineConfigBuilder.build(earlybirdCandidatePipelineScope)
  
  +  private val followingWhoToFollowCandidatePipelineConfig =
  
  +    followingWhoToFollowCandidatePipelineConfigBuilder.build(earlybirdCandidatePipelineScope)
  
   
  
     private val flipPromptCandidatePipelineConfig =
  
       flipPromptDependentCandidatePipelineConfigBuilder.build[FollowingQuery](
  
     ] = Seq(
  
       conversationServiceCandidatePipelineConfig,
  
       followingAdsCandidatePipelineConfig,
  
  -    followingWhoToFollowArmCandidatePipelineConfig,
  
  +    followingWhoToFollowCandidatePipelineConfig,
  
       flipPromptCandidatePipelineConfig,
  
       editedTweetsCandidatePipelineConfig,
  
       newTweetsPillCandidatePipelineConfig
  
   
  
     override val failOpenPolicies: Map[CandidatePipelineIdentifier, FailOpenPolicy] = Map(
  
       followingAdsCandidatePipelineConfig.identifier -> FailOpenPolicy.Always,
  
  -    followingWhoToFollowArmCandidatePipelineConfig.identifier -> FailOpenPolicy.Always,
  
  +    followingWhoToFollowCandidatePipelineConfig.identifier -> FailOpenPolicy.Always,
  
       flipPromptCandidatePipelineConfig.identifier -> FailOpenPolicy.Always,
  
       editedTweetsCandidatePipelineConfig.identifier -> FailOpenPolicy.Always,
  
       newTweetsPillCandidatePipelineConfig.identifier -> FailOpenPolicy.Always,
  
         maxSelectionsParam = ServerMaxResultsParam
  
       ),
  
       DropModuleTooFewModuleItemResults(
  
  -      candidatePipeline = followingWhoToFollowArmCandidatePipelineConfig.identifier,
  
  +      candidatePipeline = followingWhoToFollowCandidatePipelineConfig.identifier,
  
         minModuleItemsParam = StaticParam(WhoToFollowArmCandidatePipelineConfig.MinCandidatesSize)
  
       ),
  
       DropMaxModuleItemCandidates(
  
  -      candidatePipeline = followingWhoToFollowArmCandidatePipelineConfig.identifier,
  
  +      candidatePipeline = followingWhoToFollowCandidatePipelineConfig.identifier,
  
         maxModuleItemsParam = StaticParam(WhoToFollowArmCandidatePipelineConfig.MaxCandidatesSize)
  
       ),
  
       InsertAppendResults(candidatePipeline = conversationServiceCandidatePipelineConfig.identifier),
  
       InsertFixedPositionResults(
  
  -      candidatePipeline = followingWhoToFollowArmCandidatePipelineConfig.identifier,
  
  +      candidatePipeline = followingWhoToFollowCandidatePipelineConfig.identifier,
  
         positionParam = WhoToFollowPositionParam
  
       ),
  
       InsertFixedPositionResults(
  
     )
  
   
  
     private val homeScribeClientEventSideEffect = HomeScribeClientEventSideEffect(
  
  +    enableScribeClientEvents = enableScribeClientEvents,
  
       logPipelinePublisher = clientEventsScribeEventPublisher,
  
       injectedTweetsCandidatePipelineIdentifiers =
  
         Seq(conversationServiceCandidatePipelineConfig.identifier),
  
  -    adsCandidatePipelineIdentifier = followingAdsCandidatePipelineConfig.identifier,
  
  +    adsCandidatePipelineIdentifier = Some(followingAdsCandidatePipelineConfig.identifier),
  
       whoToFollowCandidatePipelineIdentifier =
  
  -      Some(followingWhoToFollowArmCandidatePipelineConfig.identifier),
  
  +      Some(followingWhoToFollowCandidatePipelineConfig.identifier),
  
     )
  
   
  
     override val resultSideEffects: Seq[PipelineResultSideEffect[FollowingQuery, Timeline]] = Seq(
  
  -    updateLastNonPollingTimeSideEffect,
  
  +    homeScribeClientEventSideEffect,
  
  +    homeTimelineServedCandidatesSideEffect,
  
       publishClientSentImpressionsEventBusSideEffect,
  
       publishClientSentImpressionsManhattanSideEffect,
  
  -    homeScribeClientEventSideEffect,
  
  -    updateTimelinesPersistenceStoreSideEffect,
  
  +    publishImpressionBloomFilterSideEffect,
  
       truncateTimelinesPersistenceStoreSideEffect,
  
  -    homeTimelineServedEntriesSideEffect
  
  +    updateLastNonPollingTimeSideEffect,
  
  +    updateTimelinesPersistenceStoreSideEffect,
  
     )
  
   
  
     override val domainMarshaller: DomainMarshaller[FollowingQuery, Timeline] = {
  
