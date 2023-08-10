a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/HomeScribeClientEventSideEffect.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/HomeScribeClientEventSideEffect.scala
==================================================

Change Range: -37,13 +52,15

Content:

::

index 83f434add..a8feef707 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/HomeScribeClientEventSideEffect.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/HomeScribeClientEventSideEffect.scala
  
   import com.twitter.home_mixer.util.CandidatesUtil
  
   import com.twitter.logpipeline.client.common.EventPublisher
  
   import com.twitter.product_mixer.component_library.side_effect.ScribeClientEventSideEffect
  
  +import com.twitter.product_mixer.core.functional_component.side_effect.PipelineResultSideEffect
  
   import com.twitter.product_mixer.core.model.common.identifier.CandidatePipelineIdentifier
  
   import com.twitter.product_mixer.core.model.common.identifier.SideEffectIdentifier
  
   import com.twitter.product_mixer.core.model.common.presentation.CandidateWithDetails
  
    * Side effect that logs served tweet metrics to Scribe as client events.
  
    */
  
   case class HomeScribeClientEventSideEffect(
  
  +  enableScribeClientEvents: Boolean,
  
     override val logPipelinePublisher: EventPublisher[LogEvent],
  
     injectedTweetsCandidatePipelineIdentifiers: Seq[CandidatePipelineIdentifier],
  
  -  adsCandidatePipelineIdentifier: CandidatePipelineIdentifier,
  
  +  adsCandidatePipelineIdentifier: Option[CandidatePipelineIdentifier] = None,
  
     whoToFollowCandidatePipelineIdentifier: Option[CandidatePipelineIdentifier] = None,
  
  -) extends ScribeClientEventSideEffect[PipelineQuery, Timeline] {
  
  +  whoToSubscribeCandidatePipelineIdentifier: Option[CandidatePipelineIdentifier] = None)
  
  +    extends ScribeClientEventSideEffect[PipelineQuery, Timeline]
  
  +    with PipelineResultSideEffect.Conditionally[
  
  +      PipelineQuery,
  
  +      Timeline
  
  +    ] {
  
   
  
     override val identifier: SideEffectIdentifier = SideEffectIdentifier("HomeScribeClientEvent")
  
   
  
     override val page = "timelinemixer"
  
   
  
  +  override def onlyIf(
  
  +    query: PipelineQuery,
  
  +    selectedCandidates: Seq[CandidateWithDetails],
  
  +    remainingCandidates: Seq[CandidateWithDetails],
  
  +    droppedCandidates: Seq[CandidateWithDetails],
  
  +    response: Timeline
  
  +  ): Boolean = enableScribeClientEvents
  
  +
  
     override def buildClientEvents(
  
       query: PipelineQuery,
  
       selectedCandidates: Seq[CandidateWithDetails],
  
       val sources = itemCandidates.groupBy(_.source)
  
       val injectedTweets =
  
         injectedTweetsCandidatePipelineIdentifiers.flatMap(sources.getOrElse(_, Seq.empty))
  
  -    val promotedTweets = sources.getOrElse(adsCandidatePipelineIdentifier, Seq.empty)
  
  +    val promotedTweets = adsCandidatePipelineIdentifier.flatMap(sources.get).toSeq.flatten
  
   
  
  -    // WhoToFollow module is not required for all home-mixer products, e.g. list tweets timeline.
  
  +    // WhoToFollow and WhoToSubscribe modules are not required for all home-mixer products, e.g. list tweets timeline.
  
       val whoToFollowUsers = whoToFollowCandidatePipelineIdentifier.flatMap(sources.get).toSeq.flatten
  
  +    val whoToSubscribeUsers =
  
  +      whoToSubscribeCandidatePipelineIdentifier.flatMap(sources.get).toSeq.flatten
  
   
  
       val servedEvents = ServedEventsBuilder
  
  -      .build(query, injectedTweets, promotedTweets, whoToFollowUsers)
  
  +      .build(query, injectedTweets, promotedTweets, whoToFollowUsers, whoToSubscribeUsers)
  
   
  
       val emptyTimelineEvents = EmptyTimelineEventsBuilder.build(query, injectedTweets)
  
   
  
