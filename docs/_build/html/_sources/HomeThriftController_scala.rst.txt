a/home-mixer/server/src/main/scala/com/twitter/home_mixer/controller/HomeThriftController.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/controller/HomeThriftController.scala
==================================================

Change Range: -7,6 +7,7

Content:

::

index fb09b405e..fc1b7770e 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/controller/HomeThriftController.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/controller/HomeThriftController.scala
  
   import com.twitter.home_mixer.{thriftscala => t}
  
   import com.twitter.product_mixer.core.controllers.DebugTwitterContext
  
   import com.twitter.product_mixer.core.functional_component.configapi.ParamsBuilder
  
  +import com.twitter.product_mixer.core.service.debug_query.DebugQueryService
  
   import com.twitter.product_mixer.core.service.urt.UrtService
  
   import com.twitter.snowflake.id.SnowflakeId
  
   import com.twitter.stitch.Stitch
  
