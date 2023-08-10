a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/subscribed/model/SubscribedQuery.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/subscribed/model/SubscribedQuery.scala
==================================================

Change Range: -0,0 +1,31

Content:

::

new file mode 100644
  
  index 000000000..2085ef54f
  
  --- /dev/null
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/subscribed/model/SubscribedQuery.scala
  
  +package com.twitter.home_mixer.product.subscribed.model
  
  +
  
  +import com.twitter.home_mixer.model.request.DeviceContext
  
  +import com.twitter.home_mixer.model.request.HasDeviceContext
  
  +import com.twitter.home_mixer.model.request.HasSeenTweetIds
  
  +import com.twitter.home_mixer.model.request.SubscribedProduct
  
  +import com.twitter.product_mixer.component_library.model.cursor.UrtOrderedCursor
  
  +import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
  +import com.twitter.product_mixer.core.model.marshalling.request._
  
  +import com.twitter.product_mixer.core.pipeline.HasPipelineCursor
  
  +import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  +import com.twitter.timelines.configapi.Params
  
  +
  
  +case class SubscribedQuery(
  
  +  override val params: Params,
  
  +  override val clientContext: ClientContext,
  
  +  override val pipelineCursor: Option[UrtOrderedCursor],
  
  +  override val requestedMaxResults: Option[Int],
  
  +  override val debugOptions: Option[DebugOptions],
  
  +  override val features: Option[FeatureMap],
  
  +  override val deviceContext: Option[DeviceContext],
  
  +  override val seenTweetIds: Option[Seq[Long]])
  
  +    extends PipelineQuery
  
  +    with HasPipelineCursor[UrtOrderedCursor]
  
  +    with HasDeviceContext
  
  +    with HasSeenTweetIds {
  
  +  override val product: Product = SubscribedProduct
  
  +
  
  +  override def withFeatureMap(features: FeatureMap): SubscribedQuery =
  
  +    copy(features = Some(features))
  
  +}
  
