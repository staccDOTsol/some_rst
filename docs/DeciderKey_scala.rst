a/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/decider/DeciderKey.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/decider/DeciderKey.scala
==================================================

Change Range: -20,21 +20,33

Content:

::

index b81ea760a..91f7646d9 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/decider/DeciderKey.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/decider/DeciderKey.scala
  
   
  
     val EnableListRecommendedUsersProduct = Value("enable_list_recommended_users_product")
  
   
  
  +  val EnableSubscribedProduct = Value("enable_subscribed_product")
  
  +
  
     // Candidate Pipelines
  
  -  val EnableForYouScoredTweetsCandidatePipeline = Value(
  
  -    "enable_for_you_scored_tweets_candidate_pipeline")
  
  +  val EnableForYouScoredTweetsCandidatePipeline =
  
  +    Value("enable_for_you_scored_tweets_candidate_pipeline")
  
  +
  
  +  val EnableScoredTweetsTweetMixerCandidatePipeline =
  
  +    Value("enable_scored_tweets_tweet_mixer_candidate_pipeline")
  
  +
  
  +  val EnableScoredTweetsInNetworkCandidatePipeline =
  
  +    Value("enable_scored_tweets_in_network_candidate_pipeline")
  
  +
  
  +  val EnableScoredTweetsUtegCandidatePipeline =
  
  +    Value("enable_scored_tweets_uteg_candidate_pipeline")
  
   
  
  -  val EnableScoredTweetsCrMixerCandidatePipeline = Value(
  
  -    "enable_scored_tweets_cr_mixer_candidate_pipeline")
  
  +  val EnableScoredTweetsFrsCandidatePipeline =
  
  +    Value("enable_scored_tweets_frs_candidate_pipeline")
  
   
  
  -  val EnableScoredTweetsFrsCandidatePipeline = Value("enable_scored_tweets_frs_candidate_pipeline")
  
  +  val EnableScoredTweetsListsCandidatePipeline =
  
  +    Value("enable_scored_tweets_lists_candidate_pipeline")
  
   
  
  -  val EnableScoredTweetsInNetworkCandidatePipeline = Value(
  
  -    "enable_scored_tweets_in_network_candidate_pipeline")
  
  +  val EnableScoredTweetsPopularVideosCandidatePipeline =
  
  +    Value("enable_scored_tweets_popular_videos_candidate_pipeline")
  
   
  
  -  val EnableScoredTweetsUtegCandidatePipeline = Value(
  
  -    "enable_scored_tweets_uteg_candidate_pipeline")
  
  +  val EnableScoredTweetsBackfillCandidatePipeline =
  
  +    Value("enable_scored_tweets_backfill_candidate_pipeline")
  
   
  
  -  val EnableSimClustersSimilarityFeatureHydration = Value(
  
  -    "enable_simclusters_similarity_feature_hydration")
  
  +  val EnableSimClustersSimilarityFeatureHydration =
  
  +    Value("enable_simclusters_similarity_feature_hydration")
  
   }
  
