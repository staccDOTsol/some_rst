a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouAdsCandidatePipelineBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouAdsCandidatePipelineBuilder.scala
==================================================

Change Range: -2,7 +2,7

Content:

::

index 82963f5bb..99ed0f584 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouAdsCandidatePipelineBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouAdsCandidatePipelineBuilder.scala
  
   
  
   import com.twitter.product_mixer.component_library.feature_hydrator.candidate.param_gated.ParamGatedCandidateFeatureHydrator
  
   import com.twitter.adserver.{thriftscala => ads}
  
  -import com.twitter.home_mixer.functional_component.decorator.HomeAdsClientEventDetailsBuilder
  
  +import com.twitter.home_mixer.functional_component.decorator.builder.HomeAdsClientEventDetailsBuilder
  
   import com.twitter.home_mixer.functional_component.gate.ExcludeSoftUserGate
  
   import com.twitter.home_mixer.param.HomeGlobalParams
  
   import com.twitter.home_mixer.param.HomeGlobalParams.EnableAdvertiserBrandSafetySettingsFeatureHydratorParam
  
