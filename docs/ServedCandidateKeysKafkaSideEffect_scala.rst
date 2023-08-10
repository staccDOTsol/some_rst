a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/ServedCandidateKeysKafkaSideEffect.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/side_effect/ServedCandidateKeysKafkaSideEffect.scala
==================================================

Change Range: -1,10 +1,10

Content:

::

similarity index 96%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/ServedCandidateKeysKafkaSideEffect.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/side_effect/ServedCandidateKeysKafkaSideEffect.scala
  
  index 28db059c8..aa1247fbb 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/ServedCandidateKeysKafkaSideEffect.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/side_effect/ServedCandidateKeysKafkaSideEffect.scala
  
  -package com.twitter.home_mixer.functional_component.side_effect
  
  +package com.twitter.home_mixer.product.for_you.side_effect
  
   
  
   import com.twitter.home_mixer.model.HomeFeatures.IsReadFromCacheFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.PredictionRequestIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.ServedIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.ServedRequestIdFeature
  
  -import com.twitter.home_mixer.param.HomeGlobalParams.EnableServedCandidateKafkaPublishingParam
  
  +import com.twitter.home_mixer.product.for_you.param.ForYouParam.EnableServedCandidateKafkaPublishingParam
  
   import com.twitter.home_mixer.service.HomeMixerAlertConfig
  
   import com.twitter.ml.api.DataRecord
  
   import com.twitter.ml.api.util.SRichDataRecord
  
