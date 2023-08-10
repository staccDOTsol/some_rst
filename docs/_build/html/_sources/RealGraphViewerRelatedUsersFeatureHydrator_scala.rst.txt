a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RealGraphViewerRelatedUsersFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/RealGraphViewerRelatedUsersFeatureHydrator.scala
==================================================

Change Range: -43,25 +44,23

Content:

::

similarity index 86%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RealGraphViewerRelatedUsersFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/RealGraphViewerRelatedUsersFeatureHydrator.scala
  
  index 45897ec05..47e2431cf 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RealGraphViewerRelatedUsersFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/RealGraphViewerRelatedUsersFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator
  
   
  
   import com.twitter.home_mixer.model.HomeFeatures.AuthorIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.DirectedAtUserIdFeature
  
   import com.twitter.product_mixer.core.functional_component.feature_hydrator.CandidateFeatureHydrator
  
   import com.twitter.product_mixer.core.model.common.identifier.FeatureHydratorIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  +import com.twitter.product_mixer.core.util.OffloadFuturePools
  
   import com.twitter.stitch.Stitch
  
   import com.twitter.timelines.prediction.adapters.real_graph.RealGraphEdgeFeaturesCombineAdapter
  
   import com.twitter.timelines.real_graph.v1.{thriftscala => v1}
  
       query: PipelineQuery,
  
       candidate: TweetCandidate,
  
       existingFeatures: FeatureMap
  
  -  ): Stitch[FeatureMap] = {
  
  +  ): Stitch[FeatureMap] = OffloadFuturePools.offload {
  
       val realGraphQueryFeatures = query.features
  
         .flatMap(_.getOrElse(RealGraphFeatures, None))
  
         .getOrElse(Map.empty[Long, v1.RealGraphEdgeFeatures])
  
   
  
       val allRelatedUserIds = getRelatedUserIds(existingFeatures)
  
  -    val realGraphFeatures =
  
  -      RealGraphViewerAuthorFeatureHydrator.getCombinedRealGraphFeatures(
  
  -        allRelatedUserIds,
  
  -        realGraphQueryFeatures)
  
  +    val realGraphFeatures = RealGraphViewerAuthorFeatureHydrator.getCombinedRealGraphFeatures(
  
  +      allRelatedUserIds,
  
  +      realGraphQueryFeatures
  
  +    )
  
       val realGraphFeaturesDataRecord = RealGraphEdgeFeaturesCombineAdapter
  
         .adaptToDataRecords(Some(realGraphFeatures)).asScala.headOption
  
         .getOrElse(new DataRecord)
  
   
  
  -    Stitch.value {
  
  -      FeatureMapBuilder()
  
  -        .add(RealGraphViewerRelatedUsersDataRecordFeature, realGraphFeaturesDataRecord)
  
  -        .build()
  
  -    }
  
  +    FeatureMapBuilder()
  
  +      .add(RealGraphViewerRelatedUsersDataRecordFeature, realGraphFeaturesDataRecord)
  
  +      .build()
  
     }
  
   
  
     private def getRelatedUserIds(features: FeatureMap): Seq[Long] = {
  
