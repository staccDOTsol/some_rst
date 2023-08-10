a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/GraphTwoHopFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/GraphTwoHopFeatureHydrator.scala
==================================================

Change Range: -87,7 +77,9

Content:

::

similarity index 76%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/GraphTwoHopFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/GraphTwoHopFeatureHydrator.scala
  
  index 7a3ed69d2..03d70445e 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/GraphTwoHopFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/GraphTwoHopFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator
  
   
  
   import com.twitter.finagle.stats.StatsReceiver
  
   import com.twitter.graph_feature_service.{thriftscala => gfs}
  
   import com.twitter.home_mixer.model.HomeFeatures.FollowedByUserIdsFeature
  
  -import com.twitter.home_mixer.model.HomeFeatures.InNetworkFeature
  
  -import com.twitter.home_mixer.model.HomeFeatures.IsExtendedReplyFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.FromInNetworkSourceFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.IsRetweetFeature
  
   import com.twitter.home_mixer.param.HomeMixerInjectionNames.GraphTwoHopRepository
  
   import com.twitter.home_mixer.util.CandidatesUtil
  
   import com.twitter.home_mixer.util.ObservedKeyValueResultHandler
  
  -import com.twitter.home_mixer.util.ReplyRetweetUtil
  
   import com.twitter.ml.api.DataRecord
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.feature.Feature
  
   import com.twitter.product_mixer.core.model.common.CandidateWithFeatures
  
   import com.twitter.product_mixer.core.model.common.identifier.FeatureHydratorIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  +import com.twitter.product_mixer.core.util.OffloadFuturePools
  
   import com.twitter.servo.repository.KeyValueRepository
  
   import com.twitter.stitch.Stitch
  
   import com.twitter.timelines.prediction.adapters.two_hop_features.TwoHopFeaturesAdapter
  
     override def apply(
  
       query: PipelineQuery,
  
       candidates: Seq[CandidateWithFeatures[TweetCandidate]]
  
  -  ): Stitch[Seq[FeatureMap]] = {
  
  -    // Apply filters to in network candidates for ExtendedReplyAncestors and retweets.
  
  -    // ExtendedReplyAncestors should also be in candidates. No filter for oon.
  
  +  ): Stitch[Seq[FeatureMap]] = OffloadFuturePools.offloadFuture {
  
  +    // Apply filters to in network candidates for retweets only.
  
       val (inNetworkCandidates, oonCandidates) = candidates.partition { candidate =>
  
  -      candidate.features.getOrElse(InNetworkFeature, false)
  
  +      candidate.features.getOrElse(FromInNetworkSourceFeature, false)
  
       }
  
   
  
  -    val inNetworkReplyToAncestorTweet =
  
  -      ReplyRetweetUtil.replyToAncestorTweetCandidatesMap(inNetworkCandidates)
  
  -
  
  -    val inNetworkExtendedReplyAncestors = inNetworkCandidates
  
  -      .filter(_.features.getOrElse(IsExtendedReplyFeature, false)).flatMap { inNetworkCandidate =>
  
  -        inNetworkReplyToAncestorTweet.get(inNetworkCandidate.candidate.id)
  
  -      }.flatten
  
  -
  
  -    val inNetworkCandidatesToHydrate = inNetworkExtendedReplyAncestors ++
  
  +    val inNetworkCandidatesToHydrate =
  
         inNetworkCandidates.filter(_.features.getOrElse(IsRetweetFeature, false))
  
   
  
       val candidatesToHydrate = (inNetworkCandidatesToHydrate ++ oonCandidates)
  
         .flatMap(candidate => CandidatesUtil.getOriginalAuthorId(candidate.features)).distinct
  
   
  
  -    val response = Stitch.callFuture(client((candidatesToHydrate, query.getRequiredUserId)))
  
  +    val response = client((candidatesToHydrate, query.getRequiredUserId))
  
   
  
       response.map { result =>
  
         candidates.map { candidate =>
  
   
  
           val value = observedGet(key = originalAuthorId, keyValueResult = result)
  
           val transformedValue = postTransformer(value)
  
  -        val followedByUserIds = value.toOption.flatMap(getFollowedByUserIds(_)).getOrElse(Seq.empty)
  
  +        val followedByUserIds = value.toOption
  
  +          .flatMap(getFollowedByUserIds(_))
  
  +          .getOrElse(Seq.empty)
  
   
  
           FeatureMapBuilder()
  
             .add(GraphTwoHopFeature, transformedValue)
  
