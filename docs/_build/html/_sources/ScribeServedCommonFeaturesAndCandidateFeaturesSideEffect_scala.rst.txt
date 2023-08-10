a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/side_effect/ScribeServedCommonFeaturesAndCandidateFeaturesSideEffect.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/side_effect/ScribeServedCommonFeaturesAndCandidateFeaturesSideEffect.scala
==================================================

Change Range: -115,6 +119,14

Content:

::

index 2599832cc..c93adee27 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/side_effect/ScribeServedCommonFeaturesAndCandidateFeaturesSideEffect.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/side_effect/ScribeServedCommonFeaturesAndCandidateFeaturesSideEffect.scala
  
   import com.twitter.finagle.mysql.Transactions
  
   import com.twitter.finagle.stats.StatsReceiver
  
   import com.twitter.finagle.util.DefaultTimer
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.adapters.non_ml_features.NonMLCandidateFeatures
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.adapters.non_ml_features.NonMLCandidateFeaturesAdapter
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.adapters.non_ml_features.NonMLCommonFeatures
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.adapters.non_ml_features.NonMLCommonFeaturesAdapter
  
   import com.twitter.home_mixer.model.HomeFeatures.ServedRequestIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.SourceTweetIdFeature
  
   import com.twitter.home_mixer.param.HomeMixerFlagName.DataRecordMetadataStoreConfigsYmlFlag
  
  +import com.twitter.home_mixer.param.HomeMixerFlagName.ScribeServedCommonFeaturesAndCandidateFeaturesFlag
  
   import com.twitter.home_mixer.param.HomeMixerInjectionNames.CandidateFeaturesScribeEventPublisher
  
   import com.twitter.home_mixer.param.HomeMixerInjectionNames.CommonFeaturesScribeEventPublisher
  
   import com.twitter.home_mixer.param.HomeMixerInjectionNames.MinimumFeaturesScribeEventPublisher
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.adapters.non_ml_features.NonMLCandidateFeatures
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.adapters.non_ml_features.NonMLCandidateFeaturesAdapter
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.adapters.non_ml_features.NonMLCommonFeatures
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.adapters.non_ml_features.NonMLCommonFeaturesAdapter
  
   import com.twitter.home_mixer.product.scored_tweets.model.ScoredTweetsQuery
  
   import com.twitter.home_mixer.product.scored_tweets.model.ScoredTweetsResponse
  
   import com.twitter.home_mixer.product.scored_tweets.scorer.CandidateFeaturesDataRecordFeature
  
   import com.twitter.home_mixer.product.scored_tweets.scorer.CommonFeaturesDataRecordFeature
  
  -import com.twitter.home_mixer.product.scored_tweets.scorer.HomeNaviModelDataRecordScorer.PredictedScoreFeatures
  
  +import com.twitter.home_mixer.product.scored_tweets.scorer.PredictedScoreFeature.PredictedScoreFeatures
  
   import com.twitter.home_mixer.util.CandidatesUtil.getOriginalAuthorId
  
   import com.twitter.inject.annotations.Flag
  
   import com.twitter.logpipeline.client.common.EventPublisher
  
   import com.twitter.product_mixer.core.feature.featuremap.datarecord.SpecificFeatures
  
   import com.twitter.product_mixer.core.functional_component.side_effect.PipelineResultSideEffect
  
   import com.twitter.product_mixer.core.model.common.identifier.SideEffectIdentifier
  
  +import com.twitter.product_mixer.core.model.common.presentation.CandidateWithDetails
  
   import com.twitter.stitch.Stitch
  
   import com.twitter.timelines.ml.cont_train.common.domain.non_scalding.CandidateAndCommonFeaturesStreamingUtils
  
   import com.twitter.timelines.ml.pldr.client.MysqlClientUtils
  
   @Singleton
  
   class ScribeServedCommonFeaturesAndCandidateFeaturesSideEffect @Inject() (
  
     @Flag(DataRecordMetadataStoreConfigsYmlFlag) dataRecordMetadataStoreConfigsYml: String,
  
  +  @Flag(ScribeServedCommonFeaturesAndCandidateFeaturesFlag) enableScribeServedCommonFeaturesAndCandidateFeatures: Boolean,
  
     @Named(CommonFeaturesScribeEventPublisher) commonFeaturesScribeEventPublisher: EventPublisher[
  
       pldr.PolyDataRecord
  
     ],
  
     ],
  
     statsReceiver: StatsReceiver,
  
   ) extends PipelineResultSideEffect[ScoredTweetsQuery, ScoredTweetsResponse]
  
  +    with PipelineResultSideEffect.Conditionally[ScoredTweetsQuery, ScoredTweetsResponse]
  
       with Logging {
  
   
  
  -  override val identifier: SideEffectIdentifier = SideEffectIdentifier(
  
  -    "ScribeServedCommonFeaturesAndCandidateFeatures")
  
  +  override val identifier: SideEffectIdentifier =
  
  +    SideEffectIdentifier("ScribeServedCommonFeaturesAndCandidateFeatures")
  
   
  
     private val drMerger = new DataRecordMerger
  
  -  private val postScoringCandidateFeatures = SpecificFeatures(PredictedScoreFeatures.toSet)
  
  -  private val postScoringCandidateFeaturesDataRecordAdapter = new DataRecordConverter(
  
  -    postScoringCandidateFeatures)
  
  +  private val postScoringCandidateFeatures = SpecificFeatures(PredictedScoreFeatures)
  
  +  private val postScoringCandidateFeaturesDataRecordAdapter =
  
  +    new DataRecordConverter(postScoringCandidateFeatures)
  
   
  
     private val scopedStatsReceiver = statsReceiver.scope(getClass.getSimpleName)
  
     private val metadataFetchFailedCounter = scopedStatsReceiver.counter("metadataFetchFailed")
  
     private val commonFeaturesScribeCounter = scopedStatsReceiver.counter("commonFeaturesScribe")
  
  -  private val commonFeaturesPLDROptionObserver = OptionObserver(
  
  -    scopedStatsReceiver.scope("commonFeaturesPLDR"))
  
  +  private val commonFeaturesPLDROptionObserver =
  
  +    OptionObserver(scopedStatsReceiver.scope("commonFeaturesPLDR"))
  
     private val candidateFeaturesScribeCounter =
  
       scopedStatsReceiver.counter("candidateFeaturesScribe")
  
  -  private val candidateFeaturesPLDROptionObserver = OptionObserver(
  
  -    scopedStatsReceiver.scope("candidateFeaturesPLDR"))
  
  -  private val minimumFeaturesPLDROptionObserver = OptionObserver(
  
  -    scopedStatsReceiver.scope("minimumFeaturesPLDR"))
  
  +  private val candidateFeaturesPLDROptionObserver =
  
  +    OptionObserver(scopedStatsReceiver.scope("candidateFeaturesPLDR"))
  
  +  private val minimumFeaturesPLDROptionObserver =
  
  +    OptionObserver(scopedStatsReceiver.scope("minimumFeaturesPLDR"))
  
     private val minimumFeaturesScribeCounter =
  
       scopedStatsReceiver.counter("minimumFeaturesScribe")
  
   
  
         )
  
     }
  
   
  
  +  override def onlyIf(
  
  +    query: ScoredTweetsQuery,
  
  +    selectedCandidates: Seq[CandidateWithDetails],
  
  +    remainingCandidates: Seq[CandidateWithDetails],
  
  +    droppedCandidates: Seq[CandidateWithDetails],
  
  +    response: ScoredTweetsResponse
  
  +  ): Boolean = enableScribeServedCommonFeaturesAndCandidateFeatures
  
  +
  
     override def apply(
  
       inputs: PipelineResultSideEffect.Inputs[ScoredTweetsQuery, ScoredTweetsResponse]
  
     ): Stitch[Unit] = {
  
