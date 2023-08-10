a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/param/ScoredTweetsParamConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/param/ScoredTweetsParamConfig.scala
==================================================

Change Range: -47,10 +70,17

Content:

::

index ae82db20f..10b9de49d 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/param/ScoredTweetsParamConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/param/ScoredTweetsParamConfig.scala
  
   import com.twitter.home_mixer.product.scored_tweets.param.ScoredTweetsParam._
  
   import com.twitter.product_mixer.core.product.ProductParamConfig
  
   import com.twitter.servo.decider.DeciderKeyName
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
     override val supportedClientFSName: String = SupportedClientFSName
  
   
  
     override val booleanDeciderOverrides = Seq(
  
  -    CrMixerSource.EnableCandidatePipelineParam,
  
  -    FrsTweetSource.EnableCandidatePipelineParam,
  
  -    InNetworkSource.EnableCandidatePipelineParam,
  
  -    UtegSource.EnableCandidatePipelineParam,
  
  +    CandidatePipeline.EnableBackfillParam,
  
  +    CandidatePipeline.EnableTweetMixerParam,
  
  +    CandidatePipeline.EnableFrsParam,
  
  +    CandidatePipeline.EnableInNetworkParam,
  
  +    CandidatePipeline.EnableListsParam,
  
  +    CandidatePipeline.EnablePopularVideosParam,
  
  +    CandidatePipeline.EnableUtegParam,
  
       ScoredTweetsParam.EnableSimClustersSimilarityFeatureHydrationDeciderParam
  
     )
  
   
  
  +  override val booleanFSOverrides = Seq(
  
  +    EnableBackfillCandidatePipelineParam,
  
  +    EnableScribeScoredCandidatesParam
  
  +  )
  
  +
  
     override val boundedIntFSOverrides = Seq(
  
       CachedScoredTweets.MinCachedTweetsParam,
  
  -    QualityFactor.CrMixerMaxTweetsToScoreParam,
  
  -    QualityFactor.MaxTweetsToScoreParam,
  
  +    MaxInNetworkResultsParam,
  
  +    MaxOutOfNetworkResultsParam,
  
  +    QualityFactor.BackfillMaxTweetsToScoreParam,
  
  +    QualityFactor.TweetMixerMaxTweetsToScoreParam,
  
  +    QualityFactor.FrsMaxTweetsToScoreParam,
  
  +    QualityFactor.InNetworkMaxTweetsToScoreParam,
  
  +    QualityFactor.ListsMaxTweetsToScoreParam,
  
  +    QualityFactor.PopularVideosMaxTweetsToScoreParam,
  
  +    QualityFactor.UtegMaxTweetsToScoreParam,
  
       ServerMaxResultsParam
  
     )
  
   
  
     )
  
   
  
     override val stringFSOverrides = Seq(
  
  -    Scoring.HomeModelParam
  
  +    Scoring.HomeModelParam,
  
  +    EarlybirdTensorflowModel.InNetworkParam,
  
  +    EarlybirdTensorflowModel.FrsParam,
  
  +    EarlybirdTensorflowModel.UtegParam
  
     )
  
   
  
     override val boundedDoubleFSOverrides = Seq(
  
  +    BlueVerifiedAuthorInNetworkMultiplierParam,
  
  +    BlueVerifiedAuthorOutOfNetworkMultiplierParam,
  
  +    CreatorInNetworkMultiplierParam,
  
  +    CreatorOutOfNetworkMultiplierParam,
  
  +    OutOfNetworkScaleFactorParam,
  
  +    // Model Weights
  
       Scoring.ModelWeights.FavParam,
  
       Scoring.ModelWeights.ReplyParam,
  
       Scoring.ModelWeights.RetweetParam,
  
       Scoring.ModelWeights.VideoPlayback50Param,
  
       Scoring.ModelWeights.ReportParam,
  
       Scoring.ModelWeights.NegativeFeedbackV2Param,
  
  +    Scoring.ModelWeights.TweetDetailDwellParam,
  
  +    Scoring.ModelWeights.ProfileDwelledParam,
  
  +    Scoring.ModelWeights.BookmarkParam,
  
  +    Scoring.ModelWeights.ShareParam,
  
  +    Scoring.ModelWeights.ShareMenuClickParam,
  
  +    Scoring.ModelWeights.StrongNegativeFeedbackParam,
  
  +    Scoring.ModelWeights.WeakNegativeFeedbackParam
  
     )
  
   
  
     override val longSetFSOverrides = Seq(
  
  -    CompetitorSetParam,
  
  +    CompetitorSetParam
  
     )
  
   
  
     override val stringSeqFSOverrides = Seq(
  
