a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/StaleTweetsCacheModule.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/StaleTweetsCacheModule.scala
==================================================

Change Range: -24,6 +24,7

Content:

::

index 070de320a..301dc51c2 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/StaleTweetsCacheModule.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/StaleTweetsCacheModule.scala
  
       MemcachedClientBuilder.buildMemcachedClient(
  
         destName = "/srv#/prod/local/cache/staletweetscache:twemcaches",
  
         numTries = 3,
  
  +      numConnections = 1,
  
         requestTimeout = 200.milliseconds,
  
         globalTimeout = 500.milliseconds,
  
         connectTimeout = 200.milliseconds,
  
