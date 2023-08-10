a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingWhoToFollowArmCandidatePipelineConfigBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingWhoToFollowCandidatePipelineConfigBuilder.scala
==================================================

Change Range: -27,7 +27,7

Content:

::

similarity index 98%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingWhoToFollowArmCandidatePipelineConfigBuilder.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingWhoToFollowCandidatePipelineConfigBuilder.scala
  
  index 484a72d5f..cd5b1795d 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingWhoToFollowArmCandidatePipelineConfigBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingWhoToFollowCandidatePipelineConfigBuilder.scala
  
   import javax.inject.Singleton
  
   
  
   @Singleton
  
  -class FollowingWhoToFollowArmCandidatePipelineConfigBuilder @Inject() (
  
  +class FollowingWhoToFollowCandidatePipelineConfigBuilder @Inject() (
  
     whoToFollowArmDependentCandidatePipelineConfigBuilder: WhoToFollowArmDependentCandidatePipelineConfigBuilder,
  
     homeWhoToFollowFeedbackActionInfoBuilder: HomeWhoToFollowFeedbackActionInfoBuilder) {
  
   
  
