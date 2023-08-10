a/home-mixer/server/src/main/scala/com/twitter/home_mixer/candidate_pipeline/EditedTweetsCandidatePipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/candidate_pipeline/EditedTweetsCandidatePipelineConfig.scala
==================================================

Change Range: -1,7 +1,7

Content:

::

index 8f824a8ff..d9bb73695 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/candidate_pipeline/EditedTweetsCandidatePipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/candidate_pipeline/EditedTweetsCandidatePipelineConfig.scala
  
   package com.twitter.home_mixer.candidate_pipeline
  
   
  
   import com.twitter.home_mixer.functional_component.candidate_source.StaleTweetsCacheCandidateSource
  
  -import com.twitter.home_mixer.functional_component.decorator.HomeFeedbackActionInfoBuilder
  
  +import com.twitter.home_mixer.functional_component.decorator.urt.builder.HomeFeedbackActionInfoBuilder
  
   import com.twitter.home_mixer.functional_component.feature_hydrator.NamesFeatureHydrator
  
   import com.twitter.home_mixer.functional_component.query_transformer.EditedTweetsCandidatePipelineQueryTransformer
  
   import com.twitter.home_mixer.service.HomeMixerAlertConfig
  
