a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/UserMetadataStoreModule.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/UserMetadataStoreModule.scala
==================================================

Change Range: -1,26 +0,0

Content:

::

deleted file mode 100644
  
  index 7a450ac81..000000000
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/UserMetadataStoreModule.scala
  
  +++ /dev/null
  
  -package com.twitter.home_mixer.module
  
  -
  
  -import com.google.inject.Provides
  
  -import com.google.inject.name.Named
  
  -import com.twitter.finagle.stats.StatsReceiver
  
  -import com.twitter.home_mixer.param.HomeMixerInjectionNames.UserMetadataManhattanEndpoint
  
  -import com.twitter.home_mixer.param.HomeMixerInjectionNames.UserLanguagesStore
  
  -import com.twitter.home_mixer.store.UserLanguagesStore
  
  -import com.twitter.inject.TwitterModule
  
  -import com.twitter.search.common.constants.{thriftscala => scc}
  
  -import com.twitter.storage.client.manhattan.kv.ManhattanKVEndpoint
  
  -import com.twitter.storehaus.ReadableStore
  
  -import javax.inject.Singleton
  
  -
  
  -object UserMetadataStoreModule extends TwitterModule {
  
  -
  
  -  @Provides
  
  -  @Singleton
  
  -  @Named(UserLanguagesStore)
  
  -  def providesUserLanguagesFeaturesStore(
  
  -    @Named(UserMetadataManhattanEndpoint) UserMetadataManhattanKVEndpoint: ManhattanKVEndpoint,
  
  -    statsReceiver: StatsReceiver
  
  -  ): ReadableStore[Long, Seq[scc.ThriftLanguage]] = {
  
  -    new UserLanguagesStore(UserMetadataManhattanKVEndpoint, statsReceiver)
  
  -  }
  
  -}
  
