a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/BaseRealTimeAggregateBulkCandidateFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/BaseRealTimeAggregateBulkCandidateFeatureHydrator.scala
==================================================

Change Range: -28,13 +29,11

Content:

::

similarity index 83%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/BaseRealTimeAggregateBulkCandidateFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/BaseRealTimeAggregateBulkCandidateFeatureHydrator.scala
  
  index d892a072a..41b565ded 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/BaseRealTimeAggregateBulkCandidateFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/BaseRealTimeAggregateBulkCandidateFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator.real_time_aggregates
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator.real_time_aggregates
  
   
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.feature.Feature
  
   import com.twitter.product_mixer.core.functional_component.feature_hydrator.BulkCandidateFeatureHydrator
  
   import com.twitter.product_mixer.core.model.common.CandidateWithFeatures
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  +import com.twitter.product_mixer.core.util.OffloadFuturePools
  
   import com.twitter.stitch.Stitch
  
   
  
   trait BaseRealTimeAggregateBulkCandidateFeatureHydrator[K]
  
     override def apply(
  
       query: PipelineQuery,
  
       candidates: Seq[CandidateWithFeatures[TweetCandidate]]
  
  -  ): Stitch[Seq[FeatureMap]] = {
  
  +  ): Stitch[Seq[FeatureMap]] = OffloadFuturePools.offloadFuture {
  
       val possiblyKeys = keysFromQueryAndCandidates(query, candidates)
  
       fetchAndConstructDataRecords(possiblyKeys).map { dataRecords =>
  
         dataRecords.map { dataRecord =>
  
  -        FeatureMapBuilder()
  
  -          .add(outputFeature, dataRecord)
  
  -          .build()
  
  +        FeatureMapBuilder().add(outputFeature, dataRecord).build()
  
         }
  
       }
  
     }
  
