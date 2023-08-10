a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/FeedbackHistoryQueryFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/FeedbackHistoryQueryFeatureHydrator.scala
==================================================

Change Range: -17,16 +15,12

Content:

::

index aae96fe00..c0793be4b 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/FeedbackHistoryQueryFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/FeedbackHistoryQueryFeatureHydrator.scala
  
   package com.twitter.home_mixer.functional_component.feature_hydrator
  
   
  
   import com.twitter.home_mixer.model.HomeFeatures.FeedbackHistoryFeature
  
  -import com.twitter.home_mixer.param.HomeGlobalParams.EnableFeedbackFatigueParam
  
   import com.twitter.product_mixer.core.feature.Feature
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMapBuilder
  
   import com.twitter.product_mixer.core.functional_component.feature_hydrator.QueryFeatureHydrator
  
  -import com.twitter.product_mixer.core.model.common.Conditionally
  
   import com.twitter.product_mixer.core.model.common.identifier.FeatureHydratorIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
   import com.twitter.stitch.Stitch
  
   @Singleton
  
   case class FeedbackHistoryQueryFeatureHydrator @Inject() (
  
     feedbackHistoryClient: FeedbackHistoryManhattanClient)
  
  -    extends QueryFeatureHydrator[PipelineQuery]
  
  -    with Conditionally[PipelineQuery] {
  
  +    extends QueryFeatureHydrator[PipelineQuery] {
  
   
  
     override val identifier: FeatureHydratorIdentifier = FeatureHydratorIdentifier("FeedbackHistory")
  
   
  
     override val features: Set[Feature[_, _]] = Set(FeedbackHistoryFeature)
  
   
  
  -  override def onlyIf(query: PipelineQuery): Boolean =
  
  -    query.params(EnableFeedbackFatigueParam)
  
  -
  
     override def hydrate(
  
       query: PipelineQuery
  
     ): Stitch[FeatureMap] =
  
