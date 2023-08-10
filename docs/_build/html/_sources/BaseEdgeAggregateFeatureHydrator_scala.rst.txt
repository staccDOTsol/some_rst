a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/offline_aggregates/BaseEdgeAggregateFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/offline_aggregates/BaseEdgeAggregateFeatureHydrator.scala
==================================================

Change Range: -74,17 +74,17

Content:

::

similarity index 88%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/offline_aggregates/BaseEdgeAggregateFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/offline_aggregates/BaseEdgeAggregateFeatureHydrator.scala
  
  index fde5ab909..43cfa967b 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/offline_aggregates/BaseEdgeAggregateFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/offline_aggregates/BaseEdgeAggregateFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator.offline_aggregates
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator.offline_aggregates
  
   
  
   import com.twitter.ml.api.DataRecord
  
   import com.twitter.ml.api.FeatureContext
  
   import com.twitter.product_mixer.core.functional_component.feature_hydrator.BulkCandidateFeatureHydrator
  
   import com.twitter.product_mixer.core.model.common.CandidateWithFeatures
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  +import com.twitter.product_mixer.core.util.OffloadFuturePools
  
   import com.twitter.stitch.Stitch
  
   import com.twitter.timelines.data_processing.ml_util.aggregation_framework.AggregateGroup
  
   import com.twitter.timelines.data_processing.ml_util.aggregation_framework.AggregateType.AggregateType
  
     override def apply(
  
       query: PipelineQuery,
  
       candidates: Seq[CandidateWithFeatures[TweetCandidate]]
  
  -  ): Stitch[Seq[FeatureMap]] = {
  
  -
  
  +  ): Stitch[Seq[FeatureMap]] = OffloadFuturePools.offload {
  
       val featureMapBuilders: Seq[FeatureMapBuilder] =
  
         for (_ <- candidates) yield FeatureMapBuilder()
  
   
  
         }
  
       }
  
   
  
  -    Stitch.value(featureMapBuilders.map(_.build()))
  
  +    featureMapBuilders.map(_.build())
  
     }
  
   
  
     private def hydrateAggregateFeature(
  
         .getOrElse(AggregateFeaturesToDecodeWithMetadata.empty)
  
   
  
       // Decode the DenseCompactDataRecords into DataRecords for each required secondary id.
  
  -    val decoded: Map[Long, DataRecord] =
  
  -      Utils.selectAndTransform(
  
  -        secondaryIds.flatten.distinct,
  
  -        featuresToDecodeWithMetadata.toDataRecord,
  
  -        extractMapFn(featuresToDecodeWithMetadata))
  
  +    val decoded: Map[Long, DataRecord] = Utils.selectAndTransform(
  
  +      secondaryIds.flatten.distinct,
  
  +      featuresToDecodeWithMetadata.toDataRecord,
  
  +      extractMapFn(featuresToDecodeWithMetadata)
  
  +    )
  
   
  
       // Remove unnecessary features in-place. This is safe because the underlying DataRecords
  
       // are unique and have just been generated in the previous step.
  
       decoded.values.foreach(Utils.filterDataRecord(_, featureContext))
  
   
  
  -    // Put features into the FeatureMapBuilder's
  
  +    // Put features into the FeatureMapBuilders
  
       secondaryIds.map { ids =>
  
         val dataRecords = ids.flatMap(decoded.get)
  
         feature.adapter.adaptToDataRecord(dataRecords)
  
