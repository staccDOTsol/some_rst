a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TweetypieContentFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/TweetypieContentFeatureHydrator.scala
==================================================

Change Range: -112,7 +108,6

Content:

::

similarity index 75%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TweetypieContentFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/TweetypieContentFeatureHydrator.scala
  
  index 8ad3acc46..93039e8bc 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TweetypieContentFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/TweetypieContentFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator
  
   
  
   import com.twitter.escherbird.{thriftscala => esb}
  
   import com.twitter.finagle.stats.Stat
  
   import com.twitter.finagle.stats.StatsReceiver
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.adapters.content.ContentFeatureAdapter
  
   import com.twitter.home_mixer.model.HomeFeatures.MediaUnderstandingAnnotationIdsFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.SourceTweetIdFeature
  
   import com.twitter.home_mixer.param.HomeMixerInjectionNames.TweetypieContentRepository
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.adapters.content.ContentFeatureAdapter
  
   import com.twitter.home_mixer.util.ObservedKeyValueResultHandler
  
   import com.twitter.home_mixer.util.tweetypie.content.FeatureExtractionHelper
  
   import com.twitter.ml.api.DataRecord
  
     private val bulkPostTransformerLatencyStat =
  
       statsReceiver.scope(statScope).scope("bulkPostTransformer").stat("latency_ms")
  
   
  
  +  private val DefaultDataRecord: DataRecord = new DataRecord()
  
  +
  
     override def apply(
  
       query: PipelineQuery,
  
       candidates: Seq[CandidateWithFeatures[TweetCandidate]]
  
  -  ): Stitch[Seq[FeatureMap]] = {
  
  -    Stitch.callFuture {
  
  -      val tweetIdsToHydrate = candidates.map(getCandidateOriginalTweetId).distinct
  
  +  ): Stitch[Seq[FeatureMap]] = OffloadFuturePools.offloadFuture {
  
  +    val tweetIdsToHydrate = candidates.map(getCandidateOriginalTweetId).distinct
  
   
  
  -      val response: Future[KeyValueResult[Long, tp.Tweet]] =
  
  -        Stat.timeFuture(bulkRequestLatencyStat)(
  
  -          if (tweetIdsToHydrate.isEmpty) {
  
  -            Future.value(KeyValueResult.empty)
  
  -          } else {
  
  -            client(tweetIdsToHydrate)
  
  -          }
  
  -        )
  
  +    val response: Future[KeyValueResult[Long, tp.Tweet]] = Stat.timeFuture(bulkRequestLatencyStat) {
  
  +      if (tweetIdsToHydrate.isEmpty) Future.value(KeyValueResult.empty)
  
  +      else client(tweetIdsToHydrate)
  
  +    }
  
   
  
  -      response.flatMap { result =>
  
  -        Stat.timeFuture(bulkPostTransformerLatencyStat) {
  
  -          OffloadFuturePools
  
  -            .parallelize[CandidateWithFeatures[TweetCandidate], Try[(Seq[Long], DataRecord)]](
  
  -              candidates,
  
  -              parTransformer(result, _),
  
  -              parallelism = 32,
  
  -              default = Return((Seq.empty, new DataRecord))
  
  -            ).map {
  
  -              _.map {
  
  -                case Return(result) =>
  
  -                  FeatureMapBuilder()
  
  -                    .add(MediaUnderstandingAnnotationIdsFeature, result._1)
  
  -                    .add(TweetypieContentDataRecordFeature, result._2)
  
  -                    .build()
  
  -                case Throw(e) =>
  
  -                  FeatureMapBuilder()
  
  -                    .add(MediaUnderstandingAnnotationIdsFeature, Throw(e))
  
  -                    .add(TweetypieContentDataRecordFeature, Throw(e))
  
  -                    .build()
  
  -              }
  
  +    response.flatMap { result =>
  
  +      Stat.timeFuture(bulkPostTransformerLatencyStat) {
  
  +        OffloadFuturePools
  
  +          .parallelize[CandidateWithFeatures[TweetCandidate], Try[(Seq[Long], DataRecord)]](
  
  +            candidates,
  
  +            parTransformer(result, _),
  
  +            parallelism = 32,
  
  +            default = Return((Seq.empty, DefaultDataRecord))
  
  +          ).map {
  
  +            _.map {
  
  +              case Return(result) =>
  
  +                FeatureMapBuilder()
  
  +                  .add(MediaUnderstandingAnnotationIdsFeature, result._1)
  
  +                  .add(TweetypieContentDataRecordFeature, result._2)
  
  +                  .build()
  
  +              case Throw(e) =>
  
  +                FeatureMapBuilder()
  
  +                  .add(MediaUnderstandingAnnotationIdsFeature, Throw(e))
  
  +                  .add(TweetypieContentDataRecordFeature, Throw(e))
  
  +                  .build()
  
               }
  
  -        }
  
  +          }
  
         }
  
       }
  
     }
  
       candidate: CandidateWithFeatures[TweetCandidate]
  
     ): Try[(Seq[Long], DataRecord)] = {
  
       val originalTweetId = Some(getCandidateOriginalTweetId(candidate))
  
  -
  
       val value = observedGet(key = originalTweetId, keyValueResult = result)
  
       Stat.time(postTransformerLatencyStat)(postTransformer(value))
  
     }
  
