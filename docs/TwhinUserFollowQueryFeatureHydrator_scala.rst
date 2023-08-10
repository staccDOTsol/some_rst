a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TwhinUserFollowQueryFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/TwhinUserFollowQueryFeatureHydrator.scala
==================================================

Change Range: -49,32 +47,24

Content:

::

similarity index 62%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TwhinUserFollowQueryFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/TwhinUserFollowQueryFeatureHydrator.scala
  
  index c0efba167..925043e1e 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TwhinUserFollowQueryFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/TwhinUserFollowQueryFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator
  
   
  
   import com.twitter.finagle.stats.StatsReceiver
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.adapters.twhin_embeddings.TwhinUserFollowEmbeddingsAdapter
  
   import com.twitter.home_mixer.param.HomeMixerInjectionNames.TwhinUserFollowFeatureRepository
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.adapters.twhin_embeddings.TwhinUserFollowEmbeddingsAdapter
  
   import com.twitter.ml.api.DataRecord
  
  -import com.twitter.ml.api.RichDataRecord
  
  -
  
  -import com.twitter.ml.api.util.ScalaToJavaDataRecordConversions
  
   import com.twitter.ml.api.{thriftscala => ml}
  
  +import com.twitter.product_mixer.core.feature.Feature
  
  +import com.twitter.product_mixer.core.feature.FeatureWithDefaultOnFailure
  
   import com.twitter.product_mixer.core.feature.datarecord.DataRecordInAFeature
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMapBuilder
  
  -import com.twitter.product_mixer.core.feature.Feature
  
  -import com.twitter.product_mixer.core.feature.FeatureWithDefaultOnFailure
  
   import com.twitter.product_mixer.core.functional_component.feature_hydrator.QueryFeatureHydrator
  
   import com.twitter.product_mixer.core.model.common.identifier.FeatureHydratorIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
   import javax.inject.Inject
  
   import javax.inject.Named
  
   import javax.inject.Singleton
  
  +import scala.collection.JavaConverters._
  
   
  
   object TwhinUserFollowFeature
  
       extends DataRecordInAFeature[PipelineQuery]
  
   
  
     override def hydrate(query: PipelineQuery): Stitch[FeatureMap] = {
  
       val userId = query.getRequiredUserId
  
  -    Stitch.callFuture(
  
  -      client(Seq(userId)).map { results =>
  
  -        val embedding: Option[ml.FloatTensor] = results(userId) match {
  
  -          case Return(value) =>
  
  -            if (value.exists(_.floats.nonEmpty)) keyFoundCounter.incr()
  
  -            else keyLossCounter.incr()
  
  -            value
  
  -          case Throw(_) =>
  
  -            keyFailureCounter.incr()
  
  -            None
  
  -          case _ =>
  
  -            None
  
  -        }
  
  -        val dataRecord =
  
  -          new RichDataRecord(new DataRecord, TwhinUserFollowEmbeddingsAdapter.getFeatureContext)
  
  -        embedding.foreach { floatTensor =>
  
  -          dataRecord.setFeatureValue(
  
  -            TwhinUserFollowEmbeddingsAdapter.twhinEmbeddingsFeature,
  
  -            ScalaToJavaDataRecordConversions.scalaTensor2Java(
  
  -              ml.GeneralTensor
  
  -                .FloatTensor(floatTensor)))
  
  -        }
  
  -        FeatureMapBuilder()
  
  -          .add(TwhinUserFollowFeature, dataRecord.getRecord)
  
  -          .build()
  
  +    Stitch.callFuture(client(Seq(userId))).map { results =>
  
  +      val embedding: Option[ml.FloatTensor] = results(userId) match {
  
  +        case Return(value) =>
  
  +          if (value.exists(_.floats.nonEmpty)) keyFoundCounter.incr()
  
  +          else keyLossCounter.incr()
  
  +          value
  
  +        case Throw(_) =>
  
  +          keyFailureCounter.incr()
  
  +          None
  
  +        case _ =>
  
  +          None
  
         }
  
  -    )
  
  +
  
  +      val dataRecord = TwhinUserFollowEmbeddingsAdapter.adaptToDataRecords(embedding).asScala.head
  
  +
  
  +      FeatureMapBuilder()
  
  +        .add(TwhinUserFollowFeature, dataRecord)
  
  +        .build()
  
  +    }
  
     }
  
   }
  
