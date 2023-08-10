a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/ImpressionBloomFilterQueryFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/ImpressionBloomFilterQueryFeatureHydrator.scala
==================================================

Change Range: -19,36 +21,39

Content:

::

index 19b1ab557..6ece16ce0 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/ImpressionBloomFilterQueryFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/ImpressionBloomFilterQueryFeatureHydrator.scala
  
   import com.twitter.conversions.DurationOps._
  
   import com.twitter.home_mixer.model.HomeFeatures.ImpressionBloomFilterFeature
  
   import com.twitter.home_mixer.model.request.HasSeenTweetIds
  
  +import com.twitter.home_mixer.param.HomeGlobalParams.ImpressionBloomFilterFalsePositiveRateParam
  
   import com.twitter.home_mixer.service.HomeMixerAlertConfig
  
   import com.twitter.product_mixer.core.feature.Feature
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
   import com.twitter.product_mixer.core.model.common.identifier.FeatureHydratorIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
   import com.twitter.stitch.Stitch
  
  -import com.twitter.timelines.impressionbloomfilter.{thriftscala => t}
  
  +import com.twitter.timelines.clients.manhattan.store.ManhattanStoreClient
  
  +import com.twitter.timelines.impressionbloomfilter.{thriftscala => blm}
  
   import com.twitter.timelines.impressionstore.impressionbloomfilter.ImpressionBloomFilter
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   @Singleton
  
   case class ImpressionBloomFilterQueryFeatureHydrator[
  
     Query <: PipelineQuery with HasSeenTweetIds] @Inject() (
  
  -  bloomFilter: ImpressionBloomFilter)
  
  -    extends QueryFeatureHydrator[Query] {
  
  +  bloomFilterClient: ManhattanStoreClient[
  
  +    blm.ImpressionBloomFilterKey,
  
  +    blm.ImpressionBloomFilterSeq
  
  +  ]) extends QueryFeatureHydrator[Query] {
  
   
  
     override val identifier: FeatureHydratorIdentifier = FeatureHydratorIdentifier(
  
       "ImpressionBloomFilter")
  
   
  
     private val ImpressionBloomFilterTTL = 7.day
  
  -  private val ImpressionBloomFilterFalsePositiveRate = 0.002
  
   
  
     override val features: Set[Feature[_, _]] = Set(ImpressionBloomFilterFeature)
  
   
  
  -  private val SurfaceArea = t.SurfaceArea.HomeTimeline
  
  +  private val SurfaceArea = blm.SurfaceArea.HomeTimeline
  
   
  
     override def hydrate(query: Query): Stitch[FeatureMap] = {
  
       val userId = query.getRequiredUserId
  
  -    bloomFilter.getBloomFilterSeq(userId, SurfaceArea).map { bloomFilterSeq =>
  
  -      val updatedBloomFilterSeq =
  
  -        if (query.seenTweetIds.forall(_.isEmpty)) bloomFilterSeq
  
  -        else {
  
  -          bloomFilter.addElements(
  
  -            userId = userId,
  
  -            surfaceArea = SurfaceArea,
  
  -            tweetIds = query.seenTweetIds.get,
  
  -            bloomFilterEntrySeq = bloomFilterSeq,
  
  -            timeToLive = ImpressionBloomFilterTTL,
  
  -            falsePositiveRate = ImpressionBloomFilterFalsePositiveRate
  
  -          )
  
  -        }
  
  -      FeatureMapBuilder().add(ImpressionBloomFilterFeature, updatedBloomFilterSeq).build()
  
  -    }
  
  +    bloomFilterClient
  
  +      .get(blm.ImpressionBloomFilterKey(userId, SurfaceArea))
  
  +      .map(_.getOrElse(blm.ImpressionBloomFilterSeq(Seq.empty)))
  
  +      .map { bloomFilterSeq =>
  
  +        val updatedBloomFilterSeq =
  
  +          if (query.seenTweetIds.forall(_.isEmpty)) bloomFilterSeq
  
  +          else {
  
  +            ImpressionBloomFilter.addSeenTweetIds(
  
  +              surfaceArea = SurfaceArea,
  
  +              tweetIds = query.seenTweetIds.get,
  
  +              bloomFilterSeq = bloomFilterSeq,
  
  +              timeToLive = ImpressionBloomFilterTTL,
  
  +              falsePositiveRate = query.params(ImpressionBloomFilterFalsePositiveRateParam)
  
  +            )
  
  +          }
  
  +        FeatureMapBuilder().add(ImpressionBloomFilterFeature, updatedBloomFilterSeq).build()
  
  +      }
  
     }
  
   
  
     override val alerts = Seq(
  
