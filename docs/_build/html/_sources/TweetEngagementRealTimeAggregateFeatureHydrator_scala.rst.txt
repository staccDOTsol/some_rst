a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/TweetEngagementRealTimeAggregateFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/TweetEngagementRealTimeAggregateFeatureHydrator.scala
==================================================

Change Range: -44,6 +56,6

Content:

::

similarity index 69%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/TweetEngagementRealTimeAggregateFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/TweetEngagementRealTimeAggregateFeatureHydrator.scala
  
  index 5a607dec2..99bae79d9 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/TweetEngagementRealTimeAggregateFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/TweetEngagementRealTimeAggregateFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator.real_time_aggregates
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator.real_time_aggregates
  
   
  
   import com.google.inject.name.Named
  
   import com.twitter.finagle.stats.StatsReceiver
  
   import com.twitter.home_mixer.param.HomeMixerInjectionNames.TweetEngagementCache
  
  +import com.twitter.home_mixer.util.CandidatesUtil
  
   import com.twitter.ml.api.DataRecord
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.feature.FeatureWithDefaultOnFailure
  
   
  
     override val aggregateGroups: Seq[AggregateGroup] = Seq(
  
       tweetEngagement30MinuteCountsProd,
  
  -    tweetEngagementTotalCountsProd
  
  +    tweetEngagementTotalCountsProd,
  
  +    tweetEngagementUserStateRealTimeAggregatesProd,
  
  +    tweetNegativeEngagementUserStateRealTimeAggregates,
  
  +    tweetNegativeEngagement6HourCounts,
  
  +    tweetNegativeEngagementTotalCounts,
  
  +    tweetShareEngagementsRealTimeAggregates,
  
  +    tweetBCEDwellEngagementsRealTimeAggregates
  
  +  )
  
  +
  
  +  override val aggregateGroupToPrefix: Map[AggregateGroup, String] = Map(
  
  +    tweetShareEngagementsRealTimeAggregates -> "original_tweet.timelines.tweet_share_engagements_real_time_aggregates.",
  
  +    tweetBCEDwellEngagementsRealTimeAggregates -> "original_tweet.timelines.tweet_bce_dwell_engagements_real_time_aggregates."
  
     )
  
   
  
     override def keysFromQueryAndCandidates(
  
       candidates: Seq[CandidateWithFeatures[TweetCandidate]]
  
     ): Seq[Option[Long]] = {
  
       candidates
  
  -      .map(candidate => Some(candidate.candidate.id))
  
  +      .map(candidate => Some(CandidatesUtil.getOriginalTweetId(candidate)))
  
     }
  
   }
  
