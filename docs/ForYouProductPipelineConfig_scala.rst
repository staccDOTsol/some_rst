a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouProductPipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouProductPipelineConfig.scala
==================================================

Change Range: -116,8 +123,8

Content:

::

index e84b05389..e1cf2161c 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouProductPipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouProductPipelineConfig.scala
  
   
  
   import com.twitter.conversions.DurationOps._
  
   import com.twitter.home_mixer.marshaller.timelines.ChronologicalCursorUnmarshaller
  
  -import com.twitter.home_mixer.model.request.HomeMixerRequest
  
   import com.twitter.home_mixer.model.request.ForYouProduct
  
   import com.twitter.home_mixer.model.request.ForYouProductContext
  
  +import com.twitter.home_mixer.model.request.HomeMixerRequest
  
   import com.twitter.home_mixer.product.for_you.model.ForYouQuery
  
  +import com.twitter.home_mixer.product.for_you.param.ForYouParam.EnablePushToHomeMixerPipelineParam
  
   import com.twitter.home_mixer.product.for_you.param.ForYouParam.EnableScoredTweetsMixerPipelineParam
  
   import com.twitter.home_mixer.product.for_you.param.ForYouParam.ServerMaxResultsParam
  
   import com.twitter.home_mixer.product.for_you.param.ForYouParamConfig
  
   class ForYouProductPipelineConfig @Inject() (
  
     forYouTimelineScorerMixerPipelineConfig: ForYouTimelineScorerMixerPipelineConfig,
  
     forYouScoredTweetsMixerPipelineConfig: ForYouScoredTweetsMixerPipelineConfig,
  
  +  forYouPushToHomeMixerPipelineConfig: ForYouPushToHomeMixerPipelineConfig,
  
     forYouParamConfig: ForYouParamConfig)
  
       extends ProductPipelineConfig[HomeMixerRequest, ForYouQuery, urt.TimelineResponse] {
  
   
  
         debugOptions = debugOptions,
  
         deviceContext = context.deviceContext,
  
         seenTweetIds = context.seenTweetIds,
  
  -      dspClientContext = context.dspClientContext
  
  +      dspClientContext = context.dspClientContext,
  
  +      pushToHomeTweetId = context.pushToHomeTweetId
  
       )
  
     }
  
   
  
  -  override val pipelines: Seq[PipelineConfig] =
  
  -    Seq(forYouTimelineScorerMixerPipelineConfig, forYouScoredTweetsMixerPipelineConfig)
  
  +  override val pipelines: Seq[PipelineConfig] = Seq(
  
  +    forYouTimelineScorerMixerPipelineConfig,
  
  +    forYouScoredTweetsMixerPipelineConfig,
  
  +    forYouPushToHomeMixerPipelineConfig
  
  +  )
  
   
  
     override def pipelineSelector(
  
       query: ForYouQuery
  
     ): ComponentIdentifier = {
  
  -    if (query.params.getBoolean(EnableScoredTweetsMixerPipelineParam))
  
  +    if (query.pushToHomeTweetId.isDefined && query.params(EnablePushToHomeMixerPipelineParam))
  
  +      forYouPushToHomeMixerPipelineConfig.identifier
  
  +    else if (query.params(EnableScoredTweetsMixerPipelineParam))
  
         forYouScoredTweetsMixerPipelineConfig.identifier
  
  -    else
  
  -      forYouTimelineScorerMixerPipelineConfig.identifier
  
  +    else forYouTimelineScorerMixerPipelineConfig.identifier
  
     }
  
   
  
     override val alerts: Seq[Alert] = Seq(
  
       LatencyAlert(
  
         notificationGroup = DefaultNotificationGroup,
  
         percentile = P99,
  
  -      warnPredicate = TriggerIfLatencyAbove(2000.millis, 15, 30),
  
  -      criticalPredicate = TriggerIfLatencyAbove(2100.millis, 15, 30)
  
  +      warnPredicate = TriggerIfLatencyAbove(2300.millis, 15, 30),
  
  +      criticalPredicate = TriggerIfLatencyAbove(2800.millis, 15, 30)
  
       ),
  
       ThroughputAlert(
  
         notificationGroup = DefaultNotificationGroup,
  
