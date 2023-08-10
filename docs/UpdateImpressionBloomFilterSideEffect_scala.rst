a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/UpdateImpressionBloomFilterSideEffect.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/PublishImpressionBloomFilterSideEffect.scala
==================================================

Change Range: -42,19 +49,17

Content:

::

similarity index 56%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/UpdateImpressionBloomFilterSideEffect.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/PublishImpressionBloomFilterSideEffect.scala
  
  index 957fbcd37..f965bafac 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/UpdateImpressionBloomFilterSideEffect.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/PublishImpressionBloomFilterSideEffect.scala
  
   package com.twitter.home_mixer.functional_component.side_effect
  
   
  
  -import com.twitter.conversions.DurationOps._
  
   import com.twitter.home_mixer.model.HomeFeatures.ImpressionBloomFilterFeature
  
   import com.twitter.home_mixer.model.request.HasSeenTweetIds
  
  +import com.twitter.home_mixer.param.HomeGlobalParams.EnableImpressionBloomFilter
  
   import com.twitter.home_mixer.service.HomeMixerAlertConfig
  
   import com.twitter.product_mixer.core.functional_component.side_effect.PipelineResultSideEffect
  
   import com.twitter.product_mixer.core.model.common.identifier.SideEffectIdentifier
  
   import com.twitter.product_mixer.core.model.common.presentation.CandidateWithDetails
  
  -import com.twitter.product_mixer.core.model.marshalling.response.urt.Timeline
  
  +import com.twitter.product_mixer.core.model.marshalling.HasMarshalling
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
   import com.twitter.stitch.Stitch
  
  -import com.twitter.timelines.impressionbloomfilter.{thriftscala => t}
  
  -import com.twitter.timelines.impressionstore.impressionbloomfilter.ImpressionBloomFilter
  
  +import com.twitter.timelines.clients.manhattan.store.ManhattanStoreClient
  
  +import com.twitter.timelines.impressionbloomfilter.{thriftscala => blm}
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
   @Singleton
  
  -class UpdateImpressionBloomFilterSideEffect @Inject() (bloomFilter: ImpressionBloomFilter)
  
  -    extends PipelineResultSideEffect[PipelineQuery with HasSeenTweetIds, Timeline]
  
  -    with PipelineResultSideEffect.Conditionally[PipelineQuery with HasSeenTweetIds, Timeline] {
  
  -
  
  -  private val SurfaceArea = t.SurfaceArea.HomeTimeline
  
  +class PublishImpressionBloomFilterSideEffect @Inject() (
  
  +  bloomFilterClient: ManhattanStoreClient[
  
  +    blm.ImpressionBloomFilterKey,
  
  +    blm.ImpressionBloomFilterSeq
  
  +  ]) extends PipelineResultSideEffect[PipelineQuery with HasSeenTweetIds, HasMarshalling]
  
  +    with PipelineResultSideEffect.Conditionally[
  
  +      PipelineQuery with HasSeenTweetIds,
  
  +      HasMarshalling
  
  +    ] {
  
   
  
     override val identifier: SideEffectIdentifier =
  
  -    SideEffectIdentifier("UpdateImpressionBloomFilter")
  
  +    SideEffectIdentifier("PublishImpressionBloomFilter")
  
  +
  
  +  private val SurfaceArea = blm.SurfaceArea.HomeTimeline
  
   
  
     override def onlyIf(
  
       query: PipelineQuery with HasSeenTweetIds,
  
       selectedCandidates: Seq[CandidateWithDetails],
  
       remainingCandidates: Seq[CandidateWithDetails],
  
       droppedCandidates: Seq[CandidateWithDetails],
  
  -    response: Timeline
  
  -  ): Boolean = query.seenTweetIds.exists(_.nonEmpty)
  
  +    response: HasMarshalling
  
  +  ): Boolean =
  
  +    query.params.getBoolean(EnableImpressionBloomFilter) && query.seenTweetIds.exists(_.nonEmpty)
  
   
  
  -  def buildEvents(query: PipelineQuery): Option[t.ImpressionBloomFilterSeq] = {
  
  +  def buildEvents(query: PipelineQuery): Option[blm.ImpressionBloomFilterSeq] = {
  
       query.features.flatMap { featureMap =>
  
         val impressionBloomFilterSeq = featureMap.get(ImpressionBloomFilterFeature)
  
         if (impressionBloomFilterSeq.entries.nonEmpty) Some(impressionBloomFilterSeq)
  
     }
  
   
  
     override def apply(
  
  -    inputs: PipelineResultSideEffect.Inputs[PipelineQuery with HasSeenTweetIds, Timeline]
  
  +    inputs: PipelineResultSideEffect.Inputs[PipelineQuery with HasSeenTweetIds, HasMarshalling]
  
     ): Stitch[Unit] = {
  
       buildEvents(inputs.query)
  
  -      .map { updatedBloomFilter =>
  
  -        bloomFilter.writeBloomFilterSeq(
  
  -          userId = inputs.query.getRequiredUserId,
  
  -          surfaceArea = SurfaceArea,
  
  -          impressionBloomFilterSeq = updatedBloomFilter)
  
  +      .map { updatedBloomFilterSeq =>
  
  +        bloomFilterClient.write(
  
  +          blm.ImpressionBloomFilterKey(inputs.query.getRequiredUserId, SurfaceArea),
  
  +          updatedBloomFilterSeq)
  
         }.getOrElse(Stitch.Unit)
  
     }
  
   
  
     override val alerts = Seq(
  
  -    HomeMixerAlertConfig.BusinessHours.defaultSuccessRateAlert(99.8),
  
  -    HomeMixerAlertConfig.BusinessHours.defaultLatencyAlert(30.millis)
  
  +    HomeMixerAlertConfig.BusinessHours.defaultSuccessRateAlert(99.8)
  
     )
  
   }
  
