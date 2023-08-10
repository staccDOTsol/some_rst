a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/ClientEventsBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/ClientEventsBuilder.scala
==================================================

Change Range: -77,6 +82,9

Content:

::

index a0233e64e..a5cf739d3 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/ClientEventsBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/ClientEventsBuilder.scala
  
   
  
   import com.twitter.conversions.DurationOps._
  
   import com.twitter.home_mixer.functional_component.decorator.HomeQueryTypePredicates
  
  -import com.twitter.home_mixer.functional_component.decorator.HomeTweetTypePredicates
  
  +import com.twitter.home_mixer.functional_component.decorator.builder.HomeTweetTypePredicates
  
   import com.twitter.home_mixer.model.HomeFeatures.AccountAgeFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.SuggestTypeFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.VideoDurationMsFeature
  
   import com.twitter.home_mixer.model.request.FollowingProduct
  
   import com.twitter.home_mixer.model.request.ForYouProduct
  
   import com.twitter.home_mixer.model.request.ListTweetsProduct
  
  +import com.twitter.home_mixer.model.request.SubscribedProduct
  
   import com.twitter.product_mixer.component_library.side_effect.ScribeClientEventSideEffect.ClientEvent
  
   import com.twitter.product_mixer.component_library.side_effect.ScribeClientEventSideEffect.EventNamespace
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
   import com.twitter.timelines.injection.scribe.InjectionScribeUtil
  
   
  
   private[side_effect] sealed trait ClientEventsBuilder {
  
  -  private val FollowingSection = Some("home_latest")
  
  +  private val FollowingSection = Some("latest")
  
     private val ForYouSection = Some("home")
  
     private val ListTweetsSection = Some("list")
  
  +  private val SubscribedSection = Some("subscribed")
  
   
  
     protected def section(query: PipelineQuery): Option[String] = {
  
       query.product match {
  
         case FollowingProduct => FollowingSection
  
         case ForYouProduct => ForYouSection
  
         case ListTweetsProduct => ListTweetsSection
  
  +      case SubscribedProduct => SubscribedSection
  
         case other => throw new UnsupportedOperationException(s"Unknown product: $other")
  
       }
  
     }
  
     private val InjectedComponent = Some("injected")
  
     private val PromotedComponent = Some("promoted")
  
     private val WhoToFollowComponent = Some("who_to_follow")
  
  +  private val WhoToSubscribeComponent = Some("who_to_subscribe")
  
     private val WithVideoDurationComponent = Some("with_video_duration")
  
     private val VideoDurationSumElement = Some("video_duration_sum")
  
     private val NumVideosElement = Some("num_videos")
  
       query: PipelineQuery,
  
       injectedTweets: Seq[ItemCandidateWithDetails],
  
       promotedTweets: Seq[ItemCandidateWithDetails],
  
  -    whoToFollowUsers: Seq[ItemCandidateWithDetails]
  
  +    whoToFollowUsers: Seq[ItemCandidateWithDetails],
  
  +    whoToSubscribeUsers: Seq[ItemCandidateWithDetails]
  
     ): Seq[ClientEvent] = {
  
       val baseEventNamespace = EventNamespace(
  
         section = section(query),
  
         ClientEvent(
  
           baseEventNamespace.copy(component = WhoToFollowComponent, action = ServedUsersAction),
  
           eventValue = count(whoToFollowUsers)),
  
  +      ClientEvent(
  
  +        baseEventNamespace.copy(component = WhoToSubscribeComponent, action = ServedUsersAction),
  
  +        eventValue = count(whoToSubscribeUsers)),
  
       )
  
   
  
       val tweetTypeServedEvents = HomeTweetTypePredicates.PredicateMap.map {
  
