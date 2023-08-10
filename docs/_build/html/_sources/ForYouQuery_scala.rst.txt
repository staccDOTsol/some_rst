a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/model/ForYouQuery.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/model/ForYouQuery.scala
==================================================

Change Range: -47,4 +48,5

Content:

::

index ec701ac60..dda427350 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/model/ForYouQuery.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/model/ForYouQuery.scala
  
     override val features: Option[FeatureMap],
  
     override val deviceContext: Option[DeviceContext],
  
     override val seenTweetIds: Option[Seq[Long]],
  
  -  override val dspClientContext: Option[dsp.DspClientContext])
  
  +  override val dspClientContext: Option[dsp.DspClientContext],
  
  +  pushToHomeTweetId: Option[Long])
  
       extends PipelineQuery
  
       with HasPipelineCursor[UrtOrderedCursor]
  
       with HasDeviceContext
  
     override val isEmptyState: Option[Boolean] = None
  
     override val isFirstRequestAfterSignup: Option[Boolean] = None
  
     override val isEndOfTimeline: Option[Boolean] = None
  
  +  override val timelineId: Option[Long] = None
  
   }
  
