a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/BaseRealtimeAggregateHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/BaseRealtimeAggregateHydrator.scala
==================================================

Change Range: -129,26 +135,4

Content:

::

similarity index 68%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/BaseRealtimeAggregateHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/BaseRealtimeAggregateHydrator.scala
  
  index 004312f2f..f97820be0 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/BaseRealtimeAggregateHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/BaseRealtimeAggregateHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator.real_time_aggregates
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator.real_time_aggregates
  
   
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.real_time_aggregates.BaseRealtimeAggregateHydrator._
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.real_time_aggregates.BaseRealtimeAggregateHydrator._
  
  +import com.twitter.home_mixer.util.DataRecordUtil
  
   import com.twitter.home_mixer.util.ObservedKeyValueResultHandler
  
   import com.twitter.ml.api.DataRecord
  
   import com.twitter.ml.api.DataRecordMerger
  
   import com.twitter.ml.api.FeatureContext
  
  +import com.twitter.ml.api.constant.SharedFeatures
  
   import com.twitter.ml.api.util.SRichDataRecord
  
   import com.twitter.ml.api.{Feature => MLApiFeature}
  
   import com.twitter.servo.cache.ReadCache
  
   import com.twitter.servo.keyvalue.KeyValueResult
  
  -import com.twitter.stitch.Stitch
  
   import com.twitter.timelines.data_processing.ml_util.aggregation_framework.AggregateGroup
  
   import com.twitter.util.Future
  
   import com.twitter.util.Time
  
   import com.twitter.util.Try
  
  -import scala.collection.JavaConverters._
  
   import java.lang.{Double => JDouble}
  
  +import scala.collection.JavaConverters._
  
   
  
   trait BaseRealtimeAggregateHydrator[K] extends ObservedKeyValueResultHandler {
  
   
  
   
  
     private lazy val featureContexts: Seq[FeatureContext] = typedAggregateGroupsList.map {
  
       typedAggregateGroups =>
  
  -      new FeatureContext(typedAggregateGroups.flatMap(_.allOutputFeatures).asJava)
  
  +      new FeatureContext(
  
  +        (SharedFeatures.TIMESTAMP +: typedAggregateGroups.flatMap(_.allOutputFeatures)).asJava
  
  +      )
  
     }
  
   
  
     private lazy val aggregateFeaturesRenameMap: Map[MLApiFeature[_], MLApiFeature[_]] = {
  
           featureContexts.zip(renamedFeatureContexts).zip(decays).foreach {
  
             case ((featureContext, renamedFeatureContext), decay) =>
  
               val decayedDr = applyDecay(dr, featureContext, decay)
  
  -            val renamedDr = applyRename(
  
  +            val renamedDr = DataRecordUtil.applyRename(
  
                 dataRecord = decayedDr,
  
                 featureContext,
  
                 renamedFeatureContext,
  
               drMerger.merge(newDr, renamedDr)
  
           }
  
           newDr
  
  -      case _ =>
  
  -        new DataRecord
  
  +      case _ => new DataRecord
  
       }
  
     }
  
   
  
  -  def fetchAndConstructDataRecords(possiblyKeys: Seq[Option[K]]): Stitch[Seq[Try[DataRecord]]] = {
  
  -    Stitch.callFuture {
  
  -      val keys = possiblyKeys.flatten
  
  +  def fetchAndConstructDataRecords(possiblyKeys: Seq[Option[K]]): Future[Seq[Try[DataRecord]]] = {
  
  +    val keys = possiblyKeys.flatten
  
   
  
  -      val response: Future[KeyValueResult[K, DataRecord]] =
  
  -        if (keys.isEmpty) {
  
  -          Future.value(KeyValueResult.empty)
  
  -        } else {
  
  -          client.get(keys)
  
  -        }
  
  +    val response: Future[KeyValueResult[K, DataRecord]] =
  
  +      if (keys.isEmpty) Future.value(KeyValueResult.empty)
  
  +      else {
  
  +        val batchResponses = keys
  
  +          .grouped(RequestBatchSize)
  
  +          .map(keyGroup => client.get(keyGroup))
  
  +          .toSeq
  
   
  
  -      response.map { result =>
  
  -        possiblyKeys.map { possiblyKey =>
  
  -          val value = observedGet(key = possiblyKey, keyValueResult = result)
  
  -          postTransformer(value)
  
  -        }
  
  +        Future.collect(batchResponses).map(_.reduce(_ ++ _))
  
  +      }
  
  +
  
  +    response.map { result =>
  
  +      possiblyKeys.map { possiblyKey =>
  
  +        val value = observedGet(key = possiblyKey, keyValueResult = result)
  
  +        postTransformer(value)
  
         }
  
       }
  
     }
  
   }
  
   
  
   object BaseRealtimeAggregateHydrator {
  
  +  private val RequestBatchSize = 5
  
  +
  
     type TimeDecay = scala.Function2[com.twitter.ml.api.DataRecord, scala.Long, scala.Unit]
  
   
  
     private def applyDecay(
  
       decay(resultDr, time)
  
       resultDr
  
     }
  
  -
  
  -  private def applyRename(
  
  -    dataRecord: DataRecord,
  
  -    featureContext: FeatureContext,
  
  -    renamedFeatureContext: FeatureContext,
  
  -    featureRenamingMap: Map[MLApiFeature[_], MLApiFeature[_]]
  
  -  ): DataRecord = {
  
  -    val richFullDr = new SRichDataRecord(dataRecord, featureContext)
  
  -    val richNewDr = new SRichDataRecord(new DataRecord, renamedFeatureContext)
  
  -    val featureIterator = featureContext.iterator()
  
  -    featureIterator.forEachRemaining { feature =>
  
  -      if (richFullDr.hasFeature(feature)) {
  
  -        val renamedFeature = featureRenamingMap.getOrElse(feature, feature)
  
  -
  
  -        val typedFeature = feature.asInstanceOf[MLApiFeature[JDouble]]
  
  -        val typedRenamedFeature = renamedFeature.asInstanceOf[MLApiFeature[JDouble]]
  
  -
  
  -        richNewDr.setFeatureValue(typedRenamedFeature, richFullDr.getFeatureValue(typedFeature))
  
  -      }
  
  -    }
  
  -    richNewDr.getRecord
  
  -  }
  
   }
  
