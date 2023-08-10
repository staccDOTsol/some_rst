a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/param/ForYouParamConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/param/ForYouParamConfig.scala
==================================================

Change Range: -17,27 +17,44

Content:

::

index ea8e389cc..001ee57ad 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/param/ForYouParamConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/param/ForYouParamConfig.scala
  
     )
  
   
  
     override val booleanFSOverrides = Seq(
  
  +    ClearCacheOnPtr.EnableParam,
  
       EnableFlipInjectionModuleCandidatePipelineParam,
  
  -    EnableWhoToFollowCandidatePipelineParam,
  
  +    EnablePushToHomeMixerPipelineParam,
  
       EnableScoredTweetsMixerPipelineParam,
  
  -    ClearCacheOnPtr.EnableParam,
  
  -    EnableTimelineScorerCandidatePipelineParam
  
  +    EnableServedCandidateKafkaPublishingParam,
  
  +    EnableTimelineScorerCandidatePipelineParam,
  
  +    EnableTopicSocialContextFilterParam,
  
  +    EnableVerifiedAuthorSocialContextBypassParam,
  
  +    EnableWhoToFollowCandidatePipelineParam,
  
  +    EnableWhoToSubscribeCandidatePipelineParam,
  
  +    EnableTweetPreviewsCandidatePipelineParam,
  
  +    EnableClearCacheOnPushToHome
  
     )
  
   
  
     override val boundedIntFSOverrides = Seq(
  
  +    AdsNumOrganicItemsParam,
  
  +    ClearCacheOnPtr.MinEntriesParam,
  
  +    FlipInlineInjectionModulePosition,
  
       ServerMaxResultsParam,
  
       WhoToFollowPositionParam,
  
  -    FlipInlineInjectionModulePosition,
  
  -    TimelineServiceMaxResultsParam,
  
  -    AdsNumOrganicItemsParam,
  
  -    ClearCacheOnPtr.MinEntriesParam
  
  +    WhoToSubscribePositionParam,
  
  +    TweetPreviewsPositionParam,
  
  +    TweetPreviewsMaxCandidatesParam
  
  +  )
  
  +
  
  +  override val stringFSOverrides = Seq(
  
  +    WhoToFollowDisplayLocationParam,
  
  +    ExperimentStatsParam
  
     )
  
   
  
     override val boundedDurationFSOverrides = Seq(
  
  -    WhoToFollowMinInjectionIntervalParam
  
  +    WhoToFollowMinInjectionIntervalParam,
  
  +    WhoToSubscribeMinInjectionIntervalParam,
  
  +    TweetPreviewsMinInjectionIntervalParam
  
     )
  
   
  
     override val enumFSOverrides = Seq(
  
  -    WhoToFollowDisplayTypeIdParam
  
  +    WhoToFollowDisplayTypeIdParam,
  
  +    WhoToSubscribeDisplayTypeIdParam
  
     )
  
   }
  
