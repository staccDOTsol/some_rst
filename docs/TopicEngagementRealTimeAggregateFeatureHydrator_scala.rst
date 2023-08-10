a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/TopicEngagementRealTimeAggregateFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/TopicEngagementRealTimeAggregateFeatureHydrator.scala
==================================================

Change Range: -36,7 +36,14

Content:

::

similarity index 79%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/TopicEngagementRealTimeAggregateFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/TopicEngagementRealTimeAggregateFeatureHydrator.scala
  
  index 6d71f45b3..12a2e1c47 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/TopicEngagementRealTimeAggregateFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/TopicEngagementRealTimeAggregateFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator.real_time_aggregates
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator.real_time_aggregates
  
   
  
  -import com.google.inject.name.Named
  
   import com.twitter.finagle.stats.StatsReceiver
  
   import com.twitter.home_mixer.model.HomeFeatures.TopicIdSocialContextFeature
  
   import com.twitter.home_mixer.param.HomeMixerInjectionNames.TopicEngagementCache
  
   import com.twitter.timelines.data_processing.ml_util.aggregation_framework.AggregateGroup
  
   import com.twitter.timelines.prediction.common.aggregates.real_time.TimelinesOnlineAggregationFeaturesOnlyConfig._
  
   import javax.inject.Inject
  
  +import javax.inject.Named
  
   import javax.inject.Singleton
  
   
  
   object TopicEngagementRealTimeAggregateFeature
  
       TopicEngagementRealTimeAggregateFeature
  
   
  
     override val aggregateGroups: Seq[AggregateGroup] = Seq(
  
  -    topicEngagementRealTimeAggregatesProd
  
  +    topicEngagementRealTimeAggregatesProd,
  
  +    topicEngagement24HourRealTimeAggregatesProd,
  
  +    topicShareEngagementsRealTimeAggregates
  
  +  )
  
  +
  
  +  override val aggregateGroupToPrefix: Map[AggregateGroup, String] = Map(
  
  +    topicEngagement24HourRealTimeAggregatesProd -> "topic.timelines.topic_engagement_24_hour_real_time_aggregates.",
  
  +    topicShareEngagementsRealTimeAggregates -> "topic.timelines.topic_share_engagements_real_time_aggregates."
  
     )
  
   
  
     override def keysFromQueryAndCandidates(
  
