a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/TweetypieStaticEntitiesCacheClientModule.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/TweetypieStaticEntitiesCacheClientModule.scala
==================================================

Change Range: -39,6 +39,7

Content:

::

index 2d0d5e08b..13283e4b3 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/TweetypieStaticEntitiesCacheClientModule.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/TweetypieStaticEntitiesCacheClientModule.scala
  
       val memCacheClient = MemcachedClientBuilder.buildMemcachedClient(
  
         destName = ProdDest,
  
         numTries = 1,
  
  +      numConnections = 1,
  
         requestTimeout = 50.milliseconds,
  
         globalTimeout = 100.milliseconds,
  
         connectTimeout = 100.milliseconds,
  
