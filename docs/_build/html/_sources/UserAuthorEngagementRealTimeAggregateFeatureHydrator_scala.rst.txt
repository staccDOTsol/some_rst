a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/UserAuthorEngagementRealTimeAggregateFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/UserAuthorEngagementRealTimeAggregateFeatureHydrator.scala
==================================================

Change Range: -51,10 +51,8

Content:

::

similarity index 90%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/UserAuthorEngagementRealTimeAggregateFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/UserAuthorEngagementRealTimeAggregateFeatureHydrator.scala
  
  index 9671c7bf8..2c13fc0f6 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/UserAuthorEngagementRealTimeAggregateFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/UserAuthorEngagementRealTimeAggregateFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator.real_time_aggregates
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator.real_time_aggregates
  
   
  
   import com.google.inject.name.Named
  
   import com.twitter.finagle.stats.StatsReceiver
  
  -import com.twitter.home_mixer.model.HomeFeatures.AuthorIdFeature
  
   import com.twitter.home_mixer.param.HomeMixerInjectionNames.UserAuthorEngagementCache
  
  +import com.twitter.home_mixer.util.CandidatesUtil
  
   import com.twitter.ml.api.DataRecord
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.feature.FeatureWithDefaultOnFailure
  
     ): Seq[Option[(Long, Long)]] = {
  
       val userId = query.getRequiredUserId
  
       candidates.map { candidate =>
  
  -      candidate.features
  
  -        .getTry(AuthorIdFeature)
  
  -        .toOption
  
  -        .flatten
  
  +      CandidatesUtil
  
  +        .getOriginalAuthorId(candidate.features)
  
           .map((userId, _))
  
       }
  
     }
  
