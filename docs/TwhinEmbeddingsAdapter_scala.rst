a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/adapters/twhin_embeddings/TwhinEmbeddingsAdapter.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/adapters/twhin_embeddings/TwhinEmbeddingsAdapter.scala
==================================================

Change Range: -19,42 +16,32

Content:

::

similarity index 63%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/adapters/twhin_embeddings/TwhinEmbeddingsAdapter.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/adapters/twhin_embeddings/TwhinEmbeddingsAdapter.scala
  
  index 99c4ba8dd..c2830e462 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/adapters/twhin_embeddings/TwhinEmbeddingsAdapter.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/adapters/twhin_embeddings/TwhinEmbeddingsAdapter.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator.adapters.twhin_embeddings
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator.adapters.twhin_embeddings
  
   
  
  -import com.twitter.ml.api.util.BufferToIterators.RichFloatBuffer
  
  -import com.twitter.ml.api.util.ScalaToJavaDataRecordConversions
  
   import com.twitter.ml.api.DataType
  
   import com.twitter.ml.api.Feature
  
   import com.twitter.ml.api.FeatureContext
  
   import com.twitter.ml.api.RichDataRecord
  
  +import com.twitter.ml.api.util.ScalaToJavaDataRecordConversions
  
   import com.twitter.ml.api.{thriftscala => ml}
  
   import com.twitter.timelines.prediction.common.adapters.TimelinesMutatingAdapterBase
  
   
  
  -import java.nio.ByteOrder
  
  -
  
  -sealed trait TwhinEmbeddingsAdapter extends TimelinesMutatingAdapterBase[Option[ml.Embedding]] {
  
  +sealed trait TwhinEmbeddingsAdapter extends TimelinesMutatingAdapterBase[Option[ml.FloatTensor]] {
  
     def twhinEmbeddingsFeature: Feature.Tensor
  
   
  
     override def getFeatureContext: FeatureContext = new FeatureContext(
  
     )
  
   
  
     override def setFeatures(
  
  -    embedding: Option[ml.Embedding],
  
  +    embedding: Option[ml.FloatTensor],
  
       richDataRecord: RichDataRecord
  
     ): Unit = {
  
  -    embedding.foreach { embedding =>
  
  -      val floatTensor = embedding.tensor map { tensor =>
  
  -        ml.FloatTensor(
  
  -          tensor.content
  
  -            .order(ByteOrder.LITTLE_ENDIAN)
  
  -            .asFloatBuffer
  
  -            .iterator.toList
  
  -            .map(_.toDouble))
  
  -      }
  
  -
  
  -      floatTensor.foreach { v =>
  
  -        richDataRecord.setFeatureValue(
  
  -          twhinEmbeddingsFeature,
  
  -          ScalaToJavaDataRecordConversions.scalaTensor2Java(ml.GeneralTensor.FloatTensor(v))
  
  -        )
  
  -      }
  
  +    embedding.foreach { floatTensor =>
  
  +      richDataRecord.setFeatureValue(
  
  +        twhinEmbeddingsFeature,
  
  +        ScalaToJavaDataRecordConversions.scalaTensor2Java(
  
  +          ml.GeneralTensor
  
  +            .FloatTensor(floatTensor)))
  
       }
  
     }
  
   }
  
   
  
   object TwhinEmbeddingsFeatures {
  
     val TwhinAuthorFollowEmbeddingsFeature: Feature.Tensor = new Feature.Tensor(
  
  -    "original_author.timelines.twhin_author_follow_embeddings.twhin_author_follow_embeddings",
  
  +    "original_author.twhin.tw_hi_n.author_follow_as_float_tensor",
  
       DataType.FLOAT
  
     )
  
   
  
     val TwhinUserEngagementEmbeddingsFeature: Feature.Tensor = new Feature.Tensor(
  
  -    "user.timelines.twhin_user_engagement_embeddings.twhin_user_engagement_embeddings",
  
  +    "user.twhin.tw_hi_n.user_engagement_as_float_tensor",
  
       DataType.FLOAT
  
     )
  
   
  
     val TwhinUserFollowEmbeddingsFeature: Feature.Tensor = new Feature.Tensor(
  
  -    "user.timelines.twhin_user_follow_embeddings.twhin_user_follow_embeddings",
  
  +    "user.twhin.tw_hi_n.user_follow_as_float_tensor",
  
       DataType.FLOAT
  
     )
  
   }
  
