a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/subscribed/param/SubscribedParamConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/subscribed/param/SubscribedParamConfig.scala
==================================================

Change Range: -0,0 +1,18

Content:

::

new file mode 100644
  
  index 000000000..58ce7ec35
  
  --- /dev/null
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/subscribed/param/SubscribedParamConfig.scala
  
  +package com.twitter.home_mixer.product.subscribed.param
  
  +
  
  +import com.twitter.home_mixer.param.decider.DeciderKey
  
  +import com.twitter.home_mixer.product.subscribed.param.SubscribedParam._
  
  +import com.twitter.product_mixer.core.product.ProductParamConfig
  
  +import com.twitter.servo.decider.DeciderKeyName
  
  +import javax.inject.Inject
  
  +import javax.inject.Singleton
  
  +
  
  +@Singleton
  
  +class SubscribedParamConfig @Inject() () extends ProductParamConfig {
  
  +  override val enabledDeciderKey: DeciderKeyName = DeciderKey.EnableSubscribedProduct
  
  +  override val supportedClientFSName: String = SupportedClientFSName
  
  +
  
  +  override val boundedIntFSOverrides = Seq(
  
  +    ServerMaxResultsParam
  
  +  )
  
  +}
  
