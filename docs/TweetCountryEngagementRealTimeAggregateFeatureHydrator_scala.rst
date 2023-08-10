a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/TweetCountryEngagementRealTimeAggregateFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/TweetCountryEngagementRealTimeAggregateFeatureHydrator.scala
==================================================

Change Range: -48,8 +51,8

Content:

::

similarity index 80%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/TweetCountryEngagementRealTimeAggregateFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/TweetCountryEngagementRealTimeAggregateFeatureHydrator.scala
  
  index 4656b1945..cd9e34a3e 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/TweetCountryEngagementRealTimeAggregateFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/TweetCountryEngagementRealTimeAggregateFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator.real_time_aggregates
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator.real_time_aggregates
  
   
  
   import com.google.inject.name.Named
  
   import com.twitter.finagle.stats.StatsReceiver
  
   import com.twitter.home_mixer.param.HomeMixerInjectionNames.TweetCountryEngagementCache
  
  +import com.twitter.home_mixer.util.CandidatesUtil
  
   import com.twitter.ml.api.DataRecord
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.feature.FeatureWithDefaultOnFailure
  
       TweetCountryEngagementRealTimeAggregateFeature
  
   
  
     override val aggregateGroups: Seq[AggregateGroup] = Seq(
  
  -    tweetCountryRealTimeAggregates
  
  +    tweetCountryRealTimeAggregates,
  
  +    tweetCountryPrivateEngagementsRealTimeAggregates
  
     )
  
   
  
     override val aggregateGroupToPrefix: Map[AggregateGroup, String] = Map(
  
  -    tweetCountryRealTimeAggregates -> "tweet-country_code.timelines.tweet_country_engagement_real_time_aggregates."
  
  +    tweetCountryRealTimeAggregates -> "tweet-country_code.timelines.tweet_country_engagement_real_time_aggregates.",
  
  +    tweetCountryPrivateEngagementsRealTimeAggregates -> "tweet-country_code.timelines.tweet_country_private_engagement_real_time_aggregates."
  
     )
  
   
  
     override def keysFromQueryAndCandidates(
  
     ): Seq[Option[(Long, String)]] = {
  
       val countryCode = query.clientContext.countryCode
  
       candidates.map { candidate =>
  
  -      val tweetId = candidate.candidate.id
  
  -      countryCode.map((tweetId, _))
  
  +      val originalTweetId = CandidatesUtil.getOriginalTweetId(candidate)
  
  +      countryCode.map((originalTweetId, _))
  
       }
  
     }
  
   }
  
