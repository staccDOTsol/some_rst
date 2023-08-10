a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/ServedCandidateFeatureKeysKafkaSideEffect.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/side_effect/ServedCandidateFeatureKeysKafkaSideEffect.scala
==================================================

Change Range: -7,7 +7,7

Content:

::

similarity index 96%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/ServedCandidateFeatureKeysKafkaSideEffect.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/side_effect/ServedCandidateFeatureKeysKafkaSideEffect.scala
  
  index 52cf0d720..bfa7dd52e 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/ServedCandidateFeatureKeysKafkaSideEffect.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/side_effect/ServedCandidateFeatureKeysKafkaSideEffect.scala
  
  -package com.twitter.home_mixer.functional_component.side_effect
  
  +package com.twitter.home_mixer.product.for_you.side_effect
  
   
  
   import com.twitter.home_mixer.model.HomeFeatures.CandidateSourceIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.InNetworkFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.ServedIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.ServedRequestIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.SuggestTypeFeature
  
  -import com.twitter.home_mixer.param.HomeGlobalParams.EnableServedCandidateKafkaPublishingParam
  
  +import com.twitter.home_mixer.product.for_you.param.ForYouParam.EnableServedCandidateKafkaPublishingParam
  
   import com.twitter.home_mixer.service.HomeMixerAlertConfig
  
   import com.twitter.product_mixer.component_library.side_effect.KafkaPublishingSideEffect
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
