a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/subscribed/param/SubscribedParam.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/subscribed/param/SubscribedParam.scala
==================================================

Change Range: -0,0 +1,15

Content:

::

new file mode 100644
  
  index 000000000..9e4ade43a
  
  --- /dev/null
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/subscribed/param/SubscribedParam.scala
  
  +package com.twitter.home_mixer.product.subscribed.param
  
  +
  
  +import com.twitter.timelines.configapi.FSBoundedParam
  
  +
  
  +object SubscribedParam {
  
  +  val SupportedClientFSName = "subscribed_supported_client"
  
  +
  
  +  object ServerMaxResultsParam
  
  +      extends FSBoundedParam[Int](
  
  +        name = "subscribed_server_max_results",
  
  +        default = 100,
  
  +        min = 1,
  
  +        max = 500
  
  +      )
  
  +}
  
