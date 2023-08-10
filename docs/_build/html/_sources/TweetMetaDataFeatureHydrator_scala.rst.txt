a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TweetMetaDataFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/TweetMetaDataFeatureHydrator.scala
==================================================

Change Range: -36,16 +37,10

Content:

::

similarity index 89%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TweetMetaDataFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/TweetMetaDataFeatureHydrator.scala
  
  index 8c79c3874..1c237bf18 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TweetMetaDataFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/TweetMetaDataFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator
  
   
  
   import com.twitter.home_mixer.model.HomeFeatures.CandidateSourceIdFeature
  
   import com.twitter.home_mixer.util.CandidatesUtil
  
   import com.twitter.product_mixer.core.functional_component.feature_hydrator.CandidateFeatureHydrator
  
   import com.twitter.product_mixer.core.model.common.identifier.FeatureHydratorIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  +import com.twitter.product_mixer.core.util.OffloadFuturePools
  
   import com.twitter.stitch.Stitch
  
   import com.twitter.timelines.prediction.features.common.TimelinesSharedFeatures
  
   import java.lang.{Long => JLong}
  
       query: PipelineQuery,
  
       candidate: TweetCandidate,
  
       existingFeatures: FeatureMap
  
  -  ): Stitch[FeatureMap] = {
  
  +  ): Stitch[FeatureMap] = OffloadFuturePools.offload {
  
       val richDataRecord = new RichDataRecord()
  
  -
  
       setFeatures(richDataRecord, candidate, existingFeatures)
  
  -
  
  -    Stitch.value {
  
  -      FeatureMapBuilder()
  
  -        .add(TweetMetaDataDataRecord, richDataRecord.getRecord)
  
  -        .build()
  
  -    }
  
  +    FeatureMapBuilder().add(TweetMetaDataDataRecord, richDataRecord.getRecord).build()
  
     }
  
   
  
     private def setFeatures(
  
