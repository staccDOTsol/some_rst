a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/PublishClientSentImpressionsEventBusSideEffect.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/PublishClientSentImpressionsEventBusSideEffect.scala
==================================================

Change Range: -56,6 +58,7

Content:

::

index 3d85d1137..c0437767e 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/PublishClientSentImpressionsEventBusSideEffect.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/PublishClientSentImpressionsEventBusSideEffect.scala
  
   import com.twitter.eventbus.client.EventBusPublisher
  
   import com.twitter.home_mixer.model.request.FollowingProduct
  
   import com.twitter.home_mixer.model.request.ForYouProduct
  
  +import com.twitter.home_mixer.model.request.SubscribedProduct
  
   import com.twitter.home_mixer.model.request.HasSeenTweetIds
  
   import com.twitter.home_mixer.service.HomeMixerAlertConfig
  
   import com.twitter.product_mixer.core.functional_component.side_effect.PipelineResultSideEffect
  
   object PublishClientSentImpressionsEventBusSideEffect {
  
     val HomeSurfaceArea: Option[Set[SurfaceArea]] = Some(Set(SurfaceArea.HomeTimeline))
  
     val HomeLatestSurfaceArea: Option[Set[SurfaceArea]] = Some(Set(SurfaceArea.HomeLatestTimeline))
  
  +  val HomeSubscribedSurfaceArea: Option[Set[SurfaceArea]] = Some(Set(SurfaceArea.HomeSubscribed))
  
   }
  
   
  
   /**
  
       val surfaceArea = query.product match {
  
         case ForYouProduct => HomeSurfaceArea
  
         case FollowingProduct => HomeLatestSurfaceArea
  
  +      case SubscribedProduct => HomeSubscribedSurfaceArea
  
         case _ => None
  
       }
  
       query.seenTweetIds.map { seenTweetIds =>
  
