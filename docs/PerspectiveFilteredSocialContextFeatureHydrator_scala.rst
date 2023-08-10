a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/PerspectiveFilteredSocialContextFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/PerspectiveFilteredSocialContextFeatureHydrator.scala
==================================================

Change Range: -59,7 +60,7

Content:

::

index 1c8dedc65..602a8b2dc 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/PerspectiveFilteredSocialContextFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/PerspectiveFilteredSocialContextFeatureHydrator.scala
  
   import com.twitter.product_mixer.core.model.common.CandidateWithFeatures
  
   import com.twitter.product_mixer.core.model.common.identifier.FeatureHydratorIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  +import com.twitter.product_mixer.core.util.OffloadFuturePools
  
   import com.twitter.stitch.Stitch
  
   import com.twitter.stitch.timelineservice.TimelineService
  
   import com.twitter.stitch.timelineservice.TimelineService.GetPerspectives
  
     override def apply(
  
       query: PipelineQuery,
  
       candidates: Seq[CandidateWithFeatures[TweetCandidate]]
  
  -  ): Stitch[Seq[FeatureMap]] = {
  
  +  ): Stitch[Seq[FeatureMap]] = OffloadFuturePools.offloadStitch {
  
       val engagingUserIdtoTweetId = candidates.flatMap { candidate =>
  
         candidate.features
  
  -        .get(FavoritedByUserIdsFeature).take(MaxCountUsers)
  
  +        .getOrElse(FavoritedByUserIdsFeature, Seq.empty).take(MaxCountUsers)
  
           .map(favoritedBy => favoritedBy -> candidate.candidate.id)
  
       }
  
   
  
   
  
         candidates.map { candidate =>
  
           val perspectiveFilteredFavoritedByUserIds: Seq[Long] = candidate.features
  
  -          .get(FavoritedByUserIdsFeature).take(MaxCountUsers)
  
  +          .getOrElse(FavoritedByUserIdsFeature, Seq.empty).take(MaxCountUsers)
  
             .filter { userId => validUserIdTweetIds.contains((userId, candidate.candidate.id)) }
  
   
  
           FeatureMapBuilder()
  
