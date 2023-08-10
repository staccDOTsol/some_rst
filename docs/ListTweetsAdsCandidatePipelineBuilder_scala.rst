a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_tweets/ListTweetsAdsCandidatePipelineBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_tweets/ListTweetsAdsCandidatePipelineBuilder.scala
==================================================

Change Range: -1,7 +1,7

Content:

::

index 77c61710a..9e5a4e541 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_tweets/ListTweetsAdsCandidatePipelineBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_tweets/ListTweetsAdsCandidatePipelineBuilder.scala
  
   package com.twitter.home_mixer.product.list_tweets
  
   
  
   import com.twitter.adserver.{thriftscala => ads}
  
  -import com.twitter.home_mixer.functional_component.decorator.HomeAdsClientEventDetailsBuilder
  
  +import com.twitter.home_mixer.functional_component.decorator.builder.HomeAdsClientEventDetailsBuilder
  
   import com.twitter.home_mixer.functional_component.gate.ExcludeSoftUserGate
  
   import com.twitter.home_mixer.param.HomeGlobalParams
  
   import com.twitter.home_mixer.param.HomeGlobalParams.EnableAdvertiserBrandSafetySettingsFeatureHydratorParam
  
