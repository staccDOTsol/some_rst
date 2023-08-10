a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/param/ScoredTweetsParam.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/param/ScoredTweetsParam.scala
==================================================

Change Range: -173,4 +296,66

Content:

::

index 261be593a..9720b9344 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/param/ScoredTweetsParam.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/param/ScoredTweetsParam.scala
  
   object ScoredTweetsParam {
  
     val SupportedClientFSName = "scored_tweets_supported_client"
  
   
  
  -  object CrMixerSource {
  
  -    object EnableCandidatePipelineParam
  
  -        extends BooleanDeciderParam(DeciderKey.EnableScoredTweetsCrMixerCandidatePipeline)
  
  -  }
  
  +  object CandidatePipeline {
  
  +    object EnableInNetworkParam
  
  +        extends BooleanDeciderParam(DeciderKey.EnableScoredTweetsInNetworkCandidatePipeline)
  
   
  
  -  object FrsTweetSource {
  
  -    object EnableCandidatePipelineParam
  
  +    object EnableTweetMixerParam
  
  +        extends BooleanDeciderParam(DeciderKey.EnableScoredTweetsTweetMixerCandidatePipeline)
  
  +
  
  +    object EnableUtegParam
  
  +        extends BooleanDeciderParam(DeciderKey.EnableScoredTweetsUtegCandidatePipeline)
  
  +
  
  +    object EnableFrsParam
  
           extends BooleanDeciderParam(DeciderKey.EnableScoredTweetsFrsCandidatePipeline)
  
  -  }
  
   
  
  -  object InNetworkSource {
  
  -    object EnableCandidatePipelineParam
  
  -        extends BooleanDeciderParam(DeciderKey.EnableScoredTweetsInNetworkCandidatePipeline)
  
  +    object EnableListsParam
  
  +        extends BooleanDeciderParam(DeciderKey.EnableScoredTweetsListsCandidatePipeline)
  
  +
  
  +    object EnablePopularVideosParam
  
  +        extends BooleanDeciderParam(DeciderKey.EnableScoredTweetsPopularVideosCandidatePipeline)
  
  +
  
  +    object EnableBackfillParam
  
  +        extends BooleanDeciderParam(DeciderKey.EnableScoredTweetsBackfillCandidatePipeline)
  
     }
  
   
  
  +  object EnableBackfillCandidatePipelineParam
  
  +      extends FSParam[Boolean](
  
  +        name = "scored_tweets_enable_backfill_candidate_pipeline",
  
  +        default = true
  
  +      )
  
  +
  
     object QualityFactor {
  
  -    object MaxTweetsToScoreParam
  
  +    object InNetworkMaxTweetsToScoreParam
  
           extends FSBoundedParam[Int](
  
  -          name = "scored_tweets_quality_factor_max_tweets_to_score",
  
  -          default = 1100,
  
  +          name = "scored_tweets_quality_factor_earlybird_max_tweets_to_score",
  
  +          default = 500,
  
             min = 0,
  
             max = 10000
  
           )
  
   
  
  -    object CrMixerMaxTweetsToScoreParam
  
  +    object UtegMaxTweetsToScoreParam
  
           extends FSBoundedParam[Int](
  
  -          name = "scored_tweets_quality_factor_cr_mixer_max_tweets_to_score",
  
  +          name = "scored_tweets_quality_factor_uteg_max_tweets_to_score",
  
  +          default = 500,
  
  +          min = 0,
  
  +          max = 10000
  
  +        )
  
  +
  
  +    object FrsMaxTweetsToScoreParam
  
  +        extends FSBoundedParam[Int](
  
  +          name = "scored_tweets_quality_factor_frs_max_tweets_to_score",
  
  +          default = 500,
  
  +          min = 0,
  
  +          max = 10000
  
  +        )
  
  +
  
  +    object TweetMixerMaxTweetsToScoreParam
  
  +        extends FSBoundedParam[Int](
  
  +          name = "scored_tweets_quality_factor_tweet_mixer_max_tweets_to_score",
  
  +          default = 500,
  
  +          min = 0,
  
  +          max = 10000
  
  +        )
  
  +
  
  +    object ListsMaxTweetsToScoreParam
  
  +        extends FSBoundedParam[Int](
  
  +          name = "scored_tweets_quality_factor_lists_max_tweets_to_score",
  
  +          default = 500,
  
  +          min = 0,
  
  +          max = 100
  
  +        )
  
  +
  
  +    object PopularVideosMaxTweetsToScoreParam
  
  +        extends FSBoundedParam[Int](
  
  +          name = "scored_tweets_quality_factor_popular_videos_max_tweets_to_score",
  
  +          default = 40,
  
  +          min = 0,
  
  +          max = 10000
  
  +        )
  
  +
  
  +    object BackfillMaxTweetsToScoreParam
  
  +        extends FSBoundedParam[Int](
  
  +          name = "scored_tweets_quality_factor_backfill_max_tweets_to_score",
  
             default = 500,
  
             min = 0,
  
             max = 10000
  
           )
  
     }
  
  +
  
     object ServerMaxResultsParam
  
         extends FSBoundedParam[Int](
  
           name = "scored_tweets_server_max_results",
  
           min = 1,
  
           max = 500
  
         )
  
  -  object UtegSource {
  
  -    object EnableCandidatePipelineParam
  
  -        extends BooleanDeciderParam(DeciderKey.EnableScoredTweetsUtegCandidatePipeline)
  
  -  }
  
  +
  
  +  object MaxInNetworkResultsParam
  
  +      extends FSBoundedParam[Int](
  
  +        name = "scored_tweets_max_in_network_results",
  
  +        default = 60,
  
  +        min = 1,
  
  +        max = 500
  
  +      )
  
  +
  
  +  object MaxOutOfNetworkResultsParam
  
  +      extends FSBoundedParam[Int](
  
  +        name = "scored_tweets_max_out_of_network_results",
  
  +        default = 60,
  
  +        min = 1,
  
  +        max = 500
  
  +      )
  
   
  
     object CachedScoredTweets {
  
       object TTLParam
  
               max = 1000000.0
  
             )
  
   
  
  +      object TweetDetailDwellParam
  
  +          extends FSBoundedParam[Double](
  
  +            name = "scored_tweets_model_weight_tweet_detail_dwell",
  
  +            default = 0.0,
  
  +            min = 0.0,
  
  +            max = 100.0
  
  +          )
  
  +
  
  +      object ProfileDwelledParam
  
  +          extends FSBoundedParam[Double](
  
  +            name = "scored_tweets_model_weight_profile_dwelled",
  
  +            default = 0.0,
  
  +            min = 0.0,
  
  +            max = 100.0
  
  +          )
  
  +
  
  +      object BookmarkParam
  
  +          extends FSBoundedParam[Double](
  
  +            name = "scored_tweets_model_weight_bookmark",
  
  +            default = 0.0,
  
  +            min = 0.0,
  
  +            max = 100.0
  
  +          )
  
  +
  
  +      object ShareParam
  
  +          extends FSBoundedParam[Double](
  
  +            name = "scored_tweets_model_weight_share",
  
  +            default = 0.0,
  
  +            min = 0.0,
  
  +            max = 100.0
  
  +          )
  
  +
  
  +      object ShareMenuClickParam
  
  +          extends FSBoundedParam[Double](
  
  +            name = "scored_tweets_model_weight_share_menu_click",
  
  +            default = 0.0,
  
  +            min = 0.0,
  
  +            max = 100.0
  
  +          )
  
  +
  
         object NegativeFeedbackV2Param
  
             extends FSBoundedParam[Double](
  
               name = "scored_tweets_model_weight_negative_feedback_v2",
  
               min = -20000.0,
  
               max = 0.0
  
             )
  
  +
  
  +      object WeakNegativeFeedbackParam
  
  +          extends FSBoundedParam[Double](
  
  +            name = "scored_tweets_model_weight_weak_negative_feedback",
  
  +            default = 0.0,
  
  +            min = -1000.0,
  
  +            max = 0.0
  
  +          )
  
  +
  
  +      object StrongNegativeFeedbackParam
  
  +          extends FSBoundedParam[Double](
  
  +            name = "scored_tweets_model_weight_strong_negative_feedback",
  
  +            default = 0.0,
  
  +            min = -1000.0,
  
  +            max = 0.0
  
  +          )
  
       }
  
     }
  
   
  
   
  
     object CompetitorURLSeqParam
  
         extends FSParam[Seq[String]](name = "scored_tweets_competitor_url_list", default = Seq.empty)
  
  +
  
  +  object BlueVerifiedAuthorInNetworkMultiplierParam
  
  +      extends FSBoundedParam[Double](
  
  +        name = "scored_tweets_blue_verified_author_in_network_multiplier",
  
  +        default = 4.0,
  
  +        min = 0.0,
  
  +        max = 100.0
  
  +      )
  
  +
  
  +  object BlueVerifiedAuthorOutOfNetworkMultiplierParam
  
  +      extends FSBoundedParam[Double](
  
  +        name = "scored_tweets_blue_verified_author_out_of_network_multiplier",
  
  +        default = 2.0,
  
  +        min = 0.0,
  
  +        max = 100.0
  
  +      )
  
  +
  
  +  object CreatorInNetworkMultiplierParam
  
  +      extends FSBoundedParam[Double](
  
  +        name = "scored_tweets_creator_in_network_multiplier",
  
  +        default = 1.1,
  
  +        min = 0.0,
  
  +        max = 100.0
  
  +      )
  
  +
  
  +  object CreatorOutOfNetworkMultiplierParam
  
  +      extends FSBoundedParam[Double](
  
  +        name = "scored_tweets_creator_out_of_network_multiplier",
  
  +        default = 1.3,
  
  +        min = 0.0,
  
  +        max = 100.0
  
  +      )
  
  +
  
  +  object OutOfNetworkScaleFactorParam
  
  +      extends FSBoundedParam[Double](
  
  +        name = "scored_tweets_out_of_network_scale_factor",
  
  +        default = 1.0,
  
  +        min = 0.0,
  
  +        max = 100.0
  
  +      )
  
  +
  
  +  object EnableScribeScoredCandidatesParam
  
  +      extends FSParam[Boolean](name = "scored_tweets_enable_scribing", default = false)
  
  +
  
  +  object EarlybirdTensorflowModel {
  
  +
  
  +    object InNetworkParam
  
  +        extends FSParam[String](
  
  +          name = "scored_tweets_in_network_earlybird_tensorflow_model",
  
  +          default = "timelines_recap_replica")
  
  +
  
  +    object FrsParam
  
  +        extends FSParam[String](
  
  +          name = "scored_tweets_frs_earlybird_tensorflow_model",
  
  +          default = "timelines_rectweet_replica")
  
  +
  
  +    object UtegParam
  
  +        extends FSParam[String](
  
  +          name = "scored_tweets_uteg_earlybird_tensorflow_model",
  
  +          default = "timelines_rectweet_replica")
  
  +  }
  
  +
  
   }
  
