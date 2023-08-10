a/home-mixer/server/src/main/scala/com/twitter/home_mixer/store/UserLanguagesStore.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/store/UserLanguagesStore.scala
==================================================

Change Range: -1,47 +0,0

Content:

::

deleted file mode 100644
  
  index 3f78ddb87..000000000
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/store/UserLanguagesStore.scala
  
  +++ /dev/null
  
  -package com.twitter.home_mixer.store
  
  -
  
  -import com.twitter.bijection.Injection
  
  -import com.twitter.finagle.stats.StatsReceiver
  
  -import com.twitter.home_mixer.store.ManhattanUserLanguagesKVDescriptor._
  
  -import com.twitter.home_mixer.util.LanguageUtil
  
  -import com.twitter.search.common.constants.{thriftscala => scc}
  
  -import com.twitter.service.metastore.gen.{thriftscala => smg}
  
  -import com.twitter.stitch.Stitch
  
  -import com.twitter.storage.client.manhattan.bijections.Bijections
  
  -import com.twitter.storage.client.manhattan.kv.ManhattanKVEndpoint
  
  -import com.twitter.storage.client.manhattan.kv.ManhattanValue
  
  -import com.twitter.storage.client.manhattan.kv.impl.ReadOnlyKeyDescriptor
  
  -import com.twitter.storage.client.manhattan.kv.impl.ValueDescriptor
  
  -import com.twitter.storehaus.ReadableStore
  
  -import com.twitter.util.Future
  
  -
  
  -object ManhattanUserLanguagesKVDescriptor {
  
  -  val userLanguagesDatasetName = "languages"
  
  -  val keyInjection = Injection.connect[Long, Array[Byte]].andThen(Bijections.BytesInjection)
  
  -  val keyDescriptor = ReadOnlyKeyDescriptor(keyInjection)
  
  -  val valueDescriptor = ValueDescriptor(Bijections.BinaryScalaInjection(smg.UserLanguages))
  
  -  val userLanguagesDatasetKey = keyDescriptor.withDataset(userLanguagesDatasetName)
  
  -}
  
  -
  
  -class UserLanguagesStore(
  
  -  manhattanKVEndpoint: ManhattanKVEndpoint,
  
  -  statsReceiver: StatsReceiver)
  
  -    extends ReadableStore[Long, Seq[scc.ThriftLanguage]] {
  
  -
  
  -  private val scopedStatsReceiver = statsReceiver.scope(getClass.getSimpleName)
  
  -  private val keyFoundCounter = scopedStatsReceiver.counter("key/found")
  
  -  private val keyLossCounter = scopedStatsReceiver.counter("key/loss")
  
  -
  
  -  override def get(viewerId: Long): Future[Option[Seq[scc.ThriftLanguage]]] =
  
  -    Stitch
  
  -      .run(
  
  -        manhattanKVEndpoint.get(key = userLanguagesDatasetKey.withPkey(viewerId), valueDescriptor))
  
  -      .map {
  
  -        case Some(mhResponse: ManhattanValue[smg.UserLanguages]) =>
  
  -          keyFoundCounter.incr()
  
  -          Some(LanguageUtil.computeLanguages(mhResponse.contents))
  
  -        case _ =>
  
  -          keyLossCounter.incr()
  
  -          None
  
  -      }
  
  -}
  
