a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TwhinAuthorFollow20220101FeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/TwhinAuthorFollowFeatureHydrator.scala
==================================================

Change Range: -87,10 +79,7

Content:

::

similarity index 57%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TwhinAuthorFollow20220101FeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/TwhinAuthorFollowFeatureHydrator.scala
  
  index 5d928a90b..2e531bfb2 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TwhinAuthorFollow20220101FeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/TwhinAuthorFollowFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator
  
   
  
   import com.twitter.finagle.stats.StatsReceiver
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.adapters.twhin_embeddings.TwhinAuthorFollowEmbeddingsAdapter
  
  -import com.twitter.home_mixer.model.HomeFeatures.AuthorIdFeature
  
  -import com.twitter.home_mixer.param.HomeMixerInjectionNames.TwhinAuthorFollow20200101FeatureRepository
  
  +import com.twitter.home_mixer.param.HomeMixerInjectionNames.TwhinAuthorFollowFeatureRepository
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.adapters.twhin_embeddings.TwhinAuthorFollowEmbeddingsAdapter
  
  +import com.twitter.home_mixer.util.CandidatesUtil
  
   import com.twitter.home_mixer.util.ObservedKeyValueResultHandler
  
   import com.twitter.ml.api.DataRecord
  
   import com.twitter.ml.api.{thriftscala => ml}
  
   import com.twitter.product_mixer.core.model.common.CandidateWithFeatures
  
   import com.twitter.product_mixer.core.model.common.identifier.FeatureHydratorIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  +import com.twitter.product_mixer.core.util.OffloadFuturePools
  
   import com.twitter.servo.repository.KeyValueRepository
  
   import com.twitter.servo.repository.KeyValueResult
  
   import com.twitter.stitch.Stitch
  
   import com.twitter.util.Future
  
   import com.twitter.util.Try
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Named
  
   import javax.inject.Singleton
  
   import scala.collection.JavaConverters._
  
   
  
  -object TwhinAuthorFollow20220101Feature
  
  +object TwhinAuthorFollowFeature
  
       extends DataRecordInAFeature[TweetCandidate]
  
       with FeatureWithDefaultOnFailure[TweetCandidate, DataRecord] {
  
     override def defaultValue: DataRecord = new DataRecord()
  
   }
  
   
  
   @Singleton
  
  -class TwhinAuthorFollow20220101FeatureHydrator @Inject() (
  
  -  @Named(TwhinAuthorFollow20200101FeatureRepository)
  
  -  client: KeyValueRepository[Seq[Long], Long, ml.Embedding],
  
  +class TwhinAuthorFollowFeatureHydrator @Inject() (
  
  +  @Named(TwhinAuthorFollowFeatureRepository)
  
  +  client: KeyValueRepository[Seq[Long], Long, ml.FloatTensor],
  
     override val statsReceiver: StatsReceiver)
  
       extends BulkCandidateFeatureHydrator[PipelineQuery, TweetCandidate]
  
       with ObservedKeyValueResultHandler {
  
   
  
     override val identifier: FeatureHydratorIdentifier =
  
  -    FeatureHydratorIdentifier("TwhinAuthorFollow20220101")
  
  +    FeatureHydratorIdentifier("TwhinAuthorFollow")
  
   
  
  -  override val features: Set[Feature[_, _]] = Set(TwhinAuthorFollow20220101Feature)
  
  +  override val features: Set[Feature[_, _]] = Set(TwhinAuthorFollowFeature)
  
   
  
     override val statScope: String = identifier.toString
  
   
  
     override def apply(
  
       query: PipelineQuery,
  
       candidates: Seq[CandidateWithFeatures[TweetCandidate]]
  
  -  ): Stitch[Seq[FeatureMap]] = {
  
  -    Stitch.callFuture {
  
  -      val possiblyAuthorIds = extractKeys(candidates)
  
  -      val authorIds = possiblyAuthorIds.flatten
  
  +  ): Stitch[Seq[FeatureMap]] = OffloadFuturePools.offloadFuture {
  
  +    val possiblyAuthorIds = extractKeys(candidates)
  
  +    val authorIds = possiblyAuthorIds.flatten
  
   
  
  -      val response: Future[KeyValueResult[Long, ml.Embedding]] =
  
  -        if (authorIds.isEmpty) {
  
  -          Future.value(KeyValueResult.empty)
  
  -        } else {
  
  -          client(authorIds)
  
  -        }
  
  +    val response: Future[KeyValueResult[Long, ml.FloatTensor]] =
  
  +      if (authorIds.isEmpty) Future.value(KeyValueResult.empty) else client(authorIds)
  
   
  
  -      response.map { result =>
  
  -        possiblyAuthorIds.map { possiblyAuthorId =>
  
  -          val value = observedGet(key = possiblyAuthorId, keyValueResult = result)
  
  -          val transformedValue = postTransformer(value)
  
  +    response.map { result =>
  
  +      possiblyAuthorIds.map { possiblyAuthorId =>
  
  +        val value = observedGet(key = possiblyAuthorId, keyValueResult = result)
  
  +        val transformedValue = postTransformer(value)
  
   
  
  -          FeatureMapBuilder()
  
  -            .add(TwhinAuthorFollow20220101Feature, transformedValue)
  
  -            .build()
  
  -        }
  
  +        FeatureMapBuilder().add(TwhinAuthorFollowFeature, transformedValue).build()
  
         }
  
       }
  
     }
  
   
  
  -  private def postTransformer(embedding: Try[Option[ml.Embedding]]): Try[DataRecord] = {
  
  -    embedding.map { e =>
  
  -      TwhinAuthorFollowEmbeddingsAdapter.adaptToDataRecords(e).asScala.head
  
  +  private def postTransformer(embedding: Try[Option[ml.FloatTensor]]): Try[DataRecord] = {
  
  +    embedding.map { floatTensor =>
  
  +      TwhinAuthorFollowEmbeddingsAdapter.adaptToDataRecords(floatTensor).asScala.head
  
       }
  
     }
  
   
  
       candidates: Seq[CandidateWithFeatures[TweetCandidate]]
  
     ): Seq[Option[Long]] = {
  
       candidates.map { candidate =>
  
  -      candidate.features
  
  -        .getTry(AuthorIdFeature)
  
  -        .toOption
  
  -        .flatten
  
  +      CandidatesUtil.getOriginalAuthorId(candidate.features)
  
       }
  
     }
  
   }
  
