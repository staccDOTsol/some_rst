a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TwhinUserEngagementQueryFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/TwhinUserEngagementQueryFeatureHydrator.scala
==================================================

Change Range: -48,33 +47,26

Content:

::

similarity index 62%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TwhinUserEngagementQueryFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/TwhinUserEngagementQueryFeatureHydrator.scala
  
  index bc602d90c..f43abc98f 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TwhinUserEngagementQueryFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/TwhinUserEngagementQueryFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator
  
   
  
   import com.twitter.finagle.stats.StatsReceiver
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.adapters.twhin_embeddings.TwhinUserEngagementEmbeddingsAdapter
  
   import com.twitter.home_mixer.param.HomeMixerInjectionNames.TwhinUserEngagementFeatureRepository
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.adapters.twhin_embeddings.TwhinUserEngagementEmbeddingsAdapter
  
   import com.twitter.ml.api.DataRecord
  
  -import com.twitter.ml.api.RichDataRecord
  
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
  
   
  
   object TwhinUserEngagementFeature
  
       extends DataRecordInAFeature[PipelineQuery]
  
   
  
     override def hydrate(query: PipelineQuery): Stitch[FeatureMap] = {
  
       val userId = query.getRequiredUserId
  
  -    Stitch.callFuture {
  
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
  
  -          new RichDataRecord(new DataRecord, TwhinUserEngagementEmbeddingsAdapter.getFeatureContext)
  
  -        embedding.foreach { floatTensor =>
  
  -          dataRecord.setFeatureValue(
  
  -            TwhinUserEngagementEmbeddingsAdapter.twhinEmbeddingsFeature,
  
  -            ScalaToJavaDataRecordConversions.scalaTensor2Java(
  
  -              ml.GeneralTensor.FloatTensor(floatTensor))
  
  -          )
  
  -        }
  
  -
  
  -        FeatureMapBuilder()
  
  -          .add(TwhinUserEngagementFeature, dataRecord.getRecord)
  
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
  
  +
  
  +      val dataRecord =
  
  +        TwhinUserEngagementEmbeddingsAdapter.adaptToDataRecords(embedding).asScala.head
  
  +
  
  +      FeatureMapBuilder()
  
  +        .add(TwhinUserEngagementFeature, dataRecord)
  
  +        .build()
  
       }
  
     }
  
  +
  
   }
  
