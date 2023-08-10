a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/UserFollowedTopicIdsFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/UserFollowedTopicIdsFeatureHydrator.scala
==================================================

Change Range: -42,27 +42,19

Content:

::

similarity index 75%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/UserFollowedTopicIdsFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/UserFollowedTopicIdsFeatureHydrator.scala
  
  index 5aed770a1..90f618fd0 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/UserFollowedTopicIdsFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/UserFollowedTopicIdsFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
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
  
   
  
  -      val response: Future[KeyValueResult[Long, Seq[Long]]] =
  
  -        if (authorIds.isEmpty) {
  
  -          Future.value(KeyValueResult.empty)
  
  -        } else {
  
  -          client(authorIds)
  
  -        }
  
  +    val response: Future[KeyValueResult[Long, Seq[Long]]] =
  
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
  
  -            .add(UserFollowedTopicIdsFeature, transformedValue)
  
  -            .build()
  
  -        }
  
  +        FeatureMapBuilder().add(UserFollowedTopicIdsFeature, transformedValue).build()
  
         }
  
       }
  
     }
  
