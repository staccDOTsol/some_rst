a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RealGraphViewerAuthorFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/RealGraphViewerAuthorFeatureHydrator.scala
==================================================

Change Range: -90,7 +91,6

Content:

::

similarity index 92%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RealGraphViewerAuthorFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/RealGraphViewerAuthorFeatureHydrator.scala
  
  index 05b434ccf..6048213aa 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RealGraphViewerAuthorFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/RealGraphViewerAuthorFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator
  
   
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.RealGraphViewerAuthorFeatureHydrator.getCombinedRealGraphFeatures
  
   import com.twitter.home_mixer.model.HomeFeatures.AuthorIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.InReplyToUserIdFeature
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.RealGraphViewerAuthorFeatureHydrator.getCombinedRealGraphFeatures
  
   import com.twitter.home_mixer.util.MissingKeyException
  
   import com.twitter.ml.api.DataRecord
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.functional_component.feature_hydrator.CandidateFeatureHydrator
  
   import com.twitter.product_mixer.core.model.common.identifier.FeatureHydratorIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  +import com.twitter.product_mixer.core.util.OffloadFuturePools
  
   import com.twitter.stitch.Stitch
  
   import com.twitter.timelines.prediction.adapters.real_graph.RealGraphEdgeFeaturesCombineAdapter
  
   import com.twitter.timelines.prediction.adapters.real_graph.RealGraphFeaturesAdapter
  
       query: PipelineQuery,
  
       candidate: TweetCandidate,
  
       existingFeatures: FeatureMap
  
  -  ): Stitch[FeatureMap] = {
  
  +  ): Stitch[FeatureMap] = OffloadFuturePools.offload {
  
       val viewerId = query.getRequiredUserId
  
       val realGraphFeatures = query.features
  
         .flatMap(_.getOrElse(RealGraphFeatures, None))
  
         .getOrElse(Map.empty[Long, v1.RealGraphEdgeFeatures])
  
   
  
  -    val result: FeatureMap = existingFeatures.getOrElse(AuthorIdFeature, None) match {
  
  +    existingFeatures.getOrElse(AuthorIdFeature, None) match {
  
         case Some(authorId) =>
  
           val realGraphAuthorFeatures =
  
             getRealGraphViewerAuthorFeatures(viewerId, authorId, realGraphFeatures)
  
             .build()
  
         case _ => MissingKeyFeatureMap
  
       }
  
  -    Stitch(result)
  
     }
  
   
  
     private def getRealGraphViewerAuthorFeatures(
  
