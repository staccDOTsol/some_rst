a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/SimClustersEngagementSimilarityFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/SimClustersEngagementSimilarityFeatureHydrator.scala
==================================================

Change Range: -77,7 +71,5

Content:

::

similarity index 85%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/SimClustersEngagementSimilarityFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/SimClustersEngagementSimilarityFeatureHydrator.scala
  
  index 0c04bce5a..f66966f00 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/SimClustersEngagementSimilarityFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/SimClustersEngagementSimilarityFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator
  
   
  
  -import com.twitter.finagle.stats.StatsReceiver
  
   import com.twitter.home_mixer.product.scored_tweets.param.ScoredTweetsParam
  
   import com.twitter.ml.api.DataRecord
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.model.common.Conditionally
  
   import com.twitter.product_mixer.core.model.common.identifier.FeatureHydratorIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  +import com.twitter.product_mixer.core.util.OffloadFuturePools
  
   import com.twitter.stitch.Stitch
  
   import com.twitter.timelines.clients.strato.twistly.SimClustersRecentEngagementSimilarityClient
  
   import com.twitter.timelines.configapi.decider.BooleanDeciderParam
  
   
  
   @Singleton
  
   class SimClustersEngagementSimilarityFeatureHydrator @Inject() (
  
  -  simClustersEngagementSimilarityClient: SimClustersRecentEngagementSimilarityClient,
  
  -  statsReceiver: StatsReceiver)
  
  +  simClustersEngagementSimilarityClient: SimClustersRecentEngagementSimilarityClient)
  
       extends BulkCandidateFeatureHydrator[PipelineQuery, TweetCandidate]
  
       with Conditionally[PipelineQuery] {
  
   
  
   
  
     override val features: Set[Feature[_, _]] = Set(SimClustersEngagementSimilarityFeature)
  
   
  
  -  private val scopedStatsReceiver = statsReceiver.scope(identifier.toString)
  
  -
  
  -  private val hydratedCandidatesSizeStat = scopedStatsReceiver.stat("hydratedCandidatesSize")
  
  -
  
     private val simClustersRecentEngagementSimilarityFeaturesAdapter =
  
       new SimClustersRecentEngagementSimilarityFeaturesAdapter
  
   
  
     override def apply(
  
       query: PipelineQuery,
  
       candidates: Seq[CandidateWithFeatures[TweetCandidate]]
  
  -  ): Stitch[Seq[FeatureMap]] = {
  
  +  ): Stitch[Seq[FeatureMap]] = OffloadFuturePools.offloadFuture {
  
       val tweetToCandidates = candidates.map(candidate => candidate.candidate.id -> candidate).toMap
  
       val tweetIds = tweetToCandidates.keySet.toSeq
  
       val userId = query.getRequiredUserId
  
       val userTweetEdges = tweetIds.map(tweetId => (userId, tweetId))
  
  -    val resultFuture = simClustersEngagementSimilarityClient
  
  +    simClustersEngagementSimilarityClient
  
         .getSimClustersRecentEngagementSimilarityScores(userTweetEdges).map {
  
           simClustersRecentEngagementSimilarityScoresMap =>
  
  -          hydratedCandidatesSizeStat.add(simClustersRecentEngagementSimilarityScoresMap.size)
  
             candidates.map { candidate =>
  
               val similarityFeatureOpt = simClustersRecentEngagementSimilarityScoresMap
  
                 .get(userId -> candidate.candidate.id).flatten
  
                 .build()
  
             }
  
         }
  
  -    Stitch.callFuture(resultFuture)
  
     }
  
  -
  
   }
  
