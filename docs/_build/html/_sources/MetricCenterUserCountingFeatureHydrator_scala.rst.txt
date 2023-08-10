a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/MetricCenterUserCountingFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/MetricCenterUserCountingFeatureHydrator.scala
==================================================

Change Range: -45,25 +44,17

Content:

::

similarity index 76%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/MetricCenterUserCountingFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/MetricCenterUserCountingFeatureHydrator.scala
  
  index de0b195e0..bc51aabb9 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/MetricCenterUserCountingFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/MetricCenterUserCountingFeatureHydrator.scala
  
  -package com.twitter.home_mixer
  
  -package functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator
  
   
  
   import com.twitter.finagle.stats.StatsReceiver
  
   import com.twitter.home_mixer.model.HomeFeatures.AuthorIdFeature
  
   import com.twitter.product_mixer.core.model.common.CandidateWithFeatures
  
   import com.twitter.product_mixer.core.model.common.identifier.FeatureHydratorIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  +import com.twitter.product_mixer.core.util.OffloadFuturePools
  
   import com.twitter.servo.keyvalue.KeyValueResult
  
   import com.twitter.servo.repository.KeyValueRepository
  
   import com.twitter.stitch.Stitch
  
   import com.twitter.util.Future
  
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
  
  -      val userIds = possiblyAuthorIds.flatten
  
  -
  
  -      val response: Future[KeyValueResult[Long, rf.MCUserCountingFeatures]] = if (userIds.isEmpty) {
  
  -        Future.value(KeyValueResult.empty)
  
  -      } else {
  
  -        client(userIds)
  
  -      }
  
  +  ): Stitch[Seq[FeatureMap]] = OffloadFuturePools.offloadFuture {
  
  +    val possiblyAuthorIds = extractKeys(candidates)
  
  +    val userIds = possiblyAuthorIds.flatten
  
   
  
  -      response.map { result =>
  
  -        possiblyAuthorIds.map { possiblyAuthorId =>
  
  -          val value = observedGet(key = possiblyAuthorId, keyValueResult = result)
  
  +    val response: Future[KeyValueResult[Long, rf.MCUserCountingFeatures]] =
  
  +      if (userIds.isEmpty) Future.value(KeyValueResult.empty) else client(userIds)
  
   
  
  -          FeatureMapBuilder()
  
  -            .add(MetricCenterUserCountingFeature, value)
  
  -            .build()
  
  -        }
  
  +    response.map { result =>
  
  +      possiblyAuthorIds.map { possiblyAuthorId =>
  
  +        val value = observedGet(key = possiblyAuthorId, keyValueResult = result)
  
  +        FeatureMapBuilder().add(MetricCenterUserCountingFeature, value).build()
  
         }
  
       }
  
     }
  
