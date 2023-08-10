a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/SGSValidSocialContextFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/SGSValidSocialContextFeatureHydrator.scala
==================================================

Change Range: -41,8 +42,7

Content:

::

index 1ba706dd1..92c351ecf 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/SGSValidSocialContextFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/SGSValidSocialContextFeatureHydrator.scala
  
   import com.twitter.product_mixer.core.model.common.CandidateWithFeatures
  
   import com.twitter.product_mixer.core.model.common.identifier.FeatureHydratorIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  +import com.twitter.product_mixer.core.util.OffloadFuturePools
  
   import com.twitter.socialgraph.{thriftscala => sg}
  
   import com.twitter.stitch.Stitch
  
   import com.twitter.stitch.socialgraph.SocialGraph
  
     override def apply(
  
       query: PipelineQuery,
  
       candidates: Seq[CandidateWithFeatures[TweetCandidate]]
  
  -  ): Stitch[Seq[FeatureMap]] = {
  
  -
  
  +  ): Stitch[Seq[FeatureMap]] = OffloadFuturePools.offloadStitch {
  
       val allSocialContextUserIds =
  
         candidates.flatMap { candidate =>
  
           candidate.features.getOrElse(FavoritedByUserIdsFeature, Nil).take(MaxCountUsers) ++
  
