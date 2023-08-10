a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingAdsCandidatePipelineBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingAdsCandidatePipelineBuilder.scala
==================================================

Change Range: -77,7 +79,11

Content:

::

index 77477652f..d4898eb31 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingAdsCandidatePipelineBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingAdsCandidatePipelineBuilder.scala
  
   package com.twitter.home_mixer.product.following
  
   
  
   import com.twitter.adserver.{thriftscala => ads}
  
  -import com.twitter.home_mixer.functional_component.decorator.HomeAdsClientEventDetailsBuilder
  
  +import com.twitter.home_mixer.functional_component.decorator.builder.HomeAdsClientEventDetailsBuilder
  
   import com.twitter.home_mixer.functional_component.gate.ExcludeSoftUserGate
  
  +import com.twitter.home_mixer.model.HomeFeatures.TweetLanguageFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.TweetTextFeature
  
   import com.twitter.home_mixer.param.HomeGlobalParams
  
   import com.twitter.home_mixer.param.HomeGlobalParams.EnableAdvertiserBrandSafetySettingsFeatureHydratorParam
  
   import com.twitter.home_mixer.product.following.model.FollowingQuery
  
   import com.twitter.product_mixer.component_library.pipeline.candidate.ads.AdsDependentCandidatePipelineConfig
  
   import com.twitter.product_mixer.component_library.pipeline.candidate.ads.AdsDependentCandidatePipelineConfigBuilder
  
   import com.twitter.product_mixer.component_library.pipeline.candidate.ads.CountCandidatesFromPipelines
  
  -import com.twitter.product_mixer.component_library.pipeline.candidate.ads.PipelineScopedOrganicItemIds
  
  +import com.twitter.product_mixer.component_library.pipeline.candidate.ads.PipelineScopedOrganicItems
  
   import com.twitter.product_mixer.component_library.pipeline.candidate.ads.ValidAdImpressionIdFilter
  
   import com.twitter.product_mixer.core.functional_component.common.CandidateScope
  
   import com.twitter.product_mixer.core.gate.ParamNotGate
  
         adsDisplayLocationBuilder = query =>
  
           if (query.params.getBoolean(EnableFastAds)) ads.DisplayLocation.TimelineHomeReverseChron
  
           else ads.DisplayLocation.TimelineHome,
  
  -      getOrganicItemIds = PipelineScopedOrganicItemIds(organicCandidatePipelines),
  
  +      getOrganicItems = PipelineScopedOrganicItems(
  
  +        pipelines = organicCandidatePipelines,
  
  +        textFeature = TweetTextFeature,
  
  +        languageFeature = TweetLanguageFeature
  
  +      ),
  
         countNumOrganicItems = CountCandidatesFromPipelines(organicCandidatePipelines),
  
         supportedClientParam = Some(EnableAdsCandidatePipelineParam),
  
         gates = Seq(
  
