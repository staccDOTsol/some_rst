a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/AuthorFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/AuthorFeatureHydrator.scala
==================================================

Change Range: -86,10 +80,7

Content:

::

similarity index 68%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/AuthorFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/AuthorFeatureHydrator.scala
  
  index a01269c2c..c8dd6a934 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/AuthorFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/AuthorFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator
  
   
  
   import com.twitter.finagle.stats.StatsReceiver
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.adapters.author_features.AuthorFeaturesAdapter
  
  -import com.twitter.home_mixer.model.HomeFeatures.AuthorIdFeature
  
   import com.twitter.home_mixer.param.HomeMixerInjectionNames.AuthorFeatureRepository
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.adapters.author_features.AuthorFeaturesAdapter
  
  +import com.twitter.home_mixer.util.CandidatesUtil
  
   import com.twitter.home_mixer.util.ObservedKeyValueResultHandler
  
   import com.twitter.ml.api.DataRecord
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
  -import com.twitter.product_mixer.core.feature.datarecord.DataRecordInAFeature
  
  -import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
   import com.twitter.product_mixer.core.feature.Feature
  
   import com.twitter.product_mixer.core.feature.FeatureWithDefaultOnFailure
  
  +import com.twitter.product_mixer.core.feature.datarecord.DataRecordInAFeature
  
  +import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMapBuilder
  
   import com.twitter.product_mixer.core.functional_component.feature_hydrator.BulkCandidateFeatureHydrator
  
   import com.twitter.product_mixer.core.model.common.CandidateWithFeatures
  
   import com.twitter.product_mixer.core.model.common.identifier.FeatureHydratorIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  -import com.twitter.servo.repository.KeyValueResult
  
  +import com.twitter.product_mixer.core.util.OffloadFuturePools
  
   import com.twitter.servo.repository.KeyValueRepository
  
  +import com.twitter.servo.repository.KeyValueResult
  
   import com.twitter.stitch.Stitch
  
   import com.twitter.timelines.author_features.v1.{thriftjava => af}
  
   import com.twitter.util.Future
  
   import com.twitter.util.Try
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Named
  
   import javax.inject.Singleton
  
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
  
   
  
  -      val response: Future[KeyValueResult[Long, af.AuthorFeatures]] =
  
  -        if (authorIds.isEmpty) {
  
  -          Future.value(KeyValueResult.empty)
  
  -        } else {
  
  -          client(authorIds)
  
  -        }
  
  +    val response: Future[KeyValueResult[Long, af.AuthorFeatures]] =
  
  +      if (authorIds.nonEmpty) client(authorIds)
  
  +      else Future.value(KeyValueResult.empty)
  
   
  
  -      response.map { result =>
  
  -        possiblyAuthorIds.map { possiblyAuthorId =>
  
  -          val value = observedGet(key = possiblyAuthorId, keyValueResult = result)
  
  -          val transformedValue = postTransformer(value)
  
  +    response.map { result =>
  
  +      possiblyAuthorIds.map { possiblyAuthorId =>
  
  +        val value = observedGet(key = possiblyAuthorId, keyValueResult = result)
  
  +        val transformedValue = postTransformer(value)
  
   
  
  -          FeatureMapBuilder()
  
  -            .add(AuthorFeature, transformedValue)
  
  -            .build()
  
  -        }
  
  +        FeatureMapBuilder().add(AuthorFeature, transformedValue).build()
  
         }
  
       }
  
     }
  
   
  
     private def postTransformer(authorFeatures: Try[Option[af.AuthorFeatures]]): Try[DataRecord] = {
  
  -    authorFeatures.map { features =>
  
  -      AuthorFeaturesAdapter.adaptToDataRecords(features).asScala.head
  
  +    authorFeatures.map {
  
  +      _.map { features => AuthorFeaturesAdapter.adaptToDataRecords(features).asScala.head }
  
  +        .getOrElse(new DataRecord())
  
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
  
