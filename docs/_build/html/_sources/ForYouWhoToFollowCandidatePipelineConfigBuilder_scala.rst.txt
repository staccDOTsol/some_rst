a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouWhoToFollowCandidatePipelineConfigBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouWhoToFollowCandidatePipelineConfigBuilder.scala
==================================================

Change Range: -48,7 +45,7

Content:

::

index 0f01c9e14..856f1fa6b 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouWhoToFollowCandidatePipelineConfigBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouWhoToFollowCandidatePipelineConfigBuilder.scala
  
   import com.twitter.home_mixer.model.HomeFeatures.WhoToFollowExcludedUserIdsFeature
  
   import com.twitter.home_mixer.product.for_you.model.ForYouQuery
  
   import com.twitter.home_mixer.product.for_you.param.ForYouParam.EnableWhoToFollowCandidatePipelineParam
  
  +import com.twitter.home_mixer.product.for_you.param.ForYouParam.WhoToFollowDisplayLocationParam
  
   import com.twitter.home_mixer.product.for_you.param.ForYouParam.WhoToFollowDisplayTypeIdParam
  
   import com.twitter.home_mixer.product.for_you.param.ForYouParam.WhoToFollowMinInjectionIntervalParam
  
   import com.twitter.home_mixer.service.HomeMixerAlertConfig
  
   import com.twitter.product_mixer.component_library.decorator.urt.builder.timeline_module.ParamWhoToFollowModuleDisplayTypeBuilder
  
  -import com.twitter.product_mixer.component_library.pipeline.candidate.who_to_follow_module.WhoToFollowCandidatePipelineConfig
  
  -import com.twitter.product_mixer.component_library.pipeline.candidate.who_to_follow_module.WhoToFollowCandidatePipelineConfigBuilder
  
  -import com.twitter.product_mixer.core.functional_component.configapi.StaticParam
  
  +import com.twitter.product_mixer.component_library.pipeline.candidate.who_to_follow_module.WhoToFollowArmCandidatePipelineConfig
  
  +import com.twitter.product_mixer.component_library.pipeline.candidate.who_to_follow_module.WhoToFollowArmCandidatePipelineConfigBuilder
  
   import com.twitter.product_mixer.core.functional_component.gate.Gate
  
   import com.twitter.timelineservice.model.rich.EntityIdType
  
   import com.twitter.timelineservice.suggests.thriftscala.SuggestType
  
   
  
   @Singleton
  
   class ForYouWhoToFollowCandidatePipelineConfigBuilder @Inject() (
  
  -  whoToFollowCandidatePipelineConfigBuilder: WhoToFollowCandidatePipelineConfigBuilder,
  
  +  whoToFollowArmCandidatePipelineConfigBuilder: WhoToFollowArmCandidatePipelineConfigBuilder,
  
     homeWhoToFollowFeedbackActionInfoBuilder: HomeWhoToFollowFeedbackActionInfoBuilder) {
  
   
  
  -  // People Discovery module timeout is set to 350ms currently so use faster display location here
  
  -  private val DisplayLocation = "timeline_reverse_chron"
  
  -
  
  -  def build(): WhoToFollowCandidatePipelineConfig[ForYouQuery] = {
  
  +  def build(): WhoToFollowArmCandidatePipelineConfig[ForYouQuery] = {
  
       val gates: Seq[Gate[ForYouQuery]] = Seq(
  
         TimelinesPersistenceStoreLastInjectionGate(
  
           WhoToFollowMinInjectionIntervalParam,
  
         DismissFatigueGate(SuggestType.WhoToFollow, DismissInfoFeature)
  
       )
  
   
  
  -    whoToFollowCandidatePipelineConfigBuilder.build[ForYouQuery](
  
  -      identifier = WhoToFollowCandidatePipelineConfig.identifier,
  
  +    whoToFollowArmCandidatePipelineConfigBuilder.build[ForYouQuery](
  
  +      identifier = WhoToFollowArmCandidatePipelineConfig.identifier,
  
         supportedClientParam = Some(EnableWhoToFollowCandidatePipelineParam),
  
         alerts = alerts,
  
         gates = gates,
  
           ParamWhoToFollowModuleDisplayTypeBuilder(WhoToFollowDisplayTypeIdParam),
  
         feedbackActionInfoBuilder = Some(homeWhoToFollowFeedbackActionInfoBuilder),
  
         excludedUserIdsFeature = Some(WhoToFollowExcludedUserIdsFeature),
  
  -      displayLocationParam = StaticParam(DisplayLocation)
  
  +      displayLocationParam = WhoToFollowDisplayLocationParam
  
       )
  
     }
  
   
  
