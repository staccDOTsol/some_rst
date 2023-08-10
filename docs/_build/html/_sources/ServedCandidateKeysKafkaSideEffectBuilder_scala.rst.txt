a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/ServedCandidateKeysKafkaSideEffectBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/side_effect/ServedCandidateKeysKafkaSideEffectBuilder.scala
==================================================

Change Range: -1,4 +1,4

Content:

::

similarity index 91%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/ServedCandidateKeysKafkaSideEffectBuilder.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/side_effect/ServedCandidateKeysKafkaSideEffectBuilder.scala
  
  index 5e86fdada..31ee389b9 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/ServedCandidateKeysKafkaSideEffectBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/side_effect/ServedCandidateKeysKafkaSideEffectBuilder.scala
  
  -package com.twitter.home_mixer.functional_component.side_effect
  
  +package com.twitter.home_mixer.product.for_you.side_effect
  
   
  
   import com.twitter.finagle.mtls.authentication.ServiceIdentifier
  
   import com.twitter.product_mixer.core.model.common.identifier.CandidatePipelineIdentifier
  
