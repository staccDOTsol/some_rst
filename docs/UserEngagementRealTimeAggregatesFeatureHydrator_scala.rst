a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/UserEngagementRealTimeAggregatesFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/UserEngagementRealTimeAggregatesFeatureHydrator.scala
==================================================

Change Range: -43,7 +43,7

Content:

::

similarity index 92%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/UserEngagementRealTimeAggregatesFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/UserEngagementRealTimeAggregatesFeatureHydrator.scala
  
  index 7526f75df..cc05b52aa 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/UserEngagementRealTimeAggregatesFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/UserEngagementRealTimeAggregatesFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator.real_time_aggregates
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator.real_time_aggregates
  
   
  
   import com.google.inject.name.Named
  
   import com.twitter.finagle.stats.StatsReceiver
  
     )
  
   
  
     override val aggregateGroupToPrefix: Map[AggregateGroup, String] = Map(
  
  -    userEngagementRealTimeAggregatesProd -> "user.timelines.user_share_engagements_real_time_aggregates.",
  
  +    userShareEngagementsRealTimeAggregates -> "user.timelines.user_share_engagements_real_time_aggregates.",
  
       userBCEDwellEngagementsRealTimeAggregates -> "user.timelines.user_bce_dwell_engagements_real_time_aggregates.",
  
       userEngagement48HourRealTimeAggregatesProd -> "user.timelines.user_engagement_48_hour_real_time_aggregates.",
  
       userNegativeEngagementAuthorUserState72HourRealTimeAggregates -> "user.timelines.user_negative_engagement_author_user_state_72_hour_real_time_aggregates.",
  
