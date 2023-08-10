a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/offline_aggregates/PartBAggregateQueryFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/offline_aggregates/PartBAggregateQueryFeatureHydrator.scala
==================================================

Change Range: -104,7 +104,7

Content:

::

similarity index 96%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/offline_aggregates/PartBAggregateQueryFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/offline_aggregates/PartBAggregateQueryFeatureHydrator.scala
  
  index 4a00c7ca4..fa0336165 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/offline_aggregates/PartBAggregateQueryFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/offline_aggregates/PartBAggregateQueryFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator.offline_aggregates
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator.offline_aggregates
  
   
  
   import com.twitter.home_mixer.param.HomeMixerInjectionNames.TimelineAggregateMetadataRepository
  
   import com.twitter.home_mixer.param.HomeMixerInjectionNames.TimelineAggregatePartBRepository
  
       aggregateGroups = Set(
  
         TimelinesAggregationConfig.userAggregatesV2,
  
         TimelinesAggregationConfig.userAggregatesV5Continuous,
  
  -      TimelinesAggregationConfig.userReciprocalEngagementAggregates,
  
  +      TimelinesAggregationConfig.userAggregatesV6,
  
         TimelinesAggregationConfig.twitterWideUserAggregates,
  
       ),
  
       aggregateType = AggregateType.User
  
               Option(featuresWithMetadata.userRequestHourAggregates.get(hourOfDay))
  
                 .map(featuresWithMetadata.toDataRecord)
  
             val userRequestDowAggregatesDr =
  
  -            Option(featuresWithMetadata.userRequestHourAggregates.get(dayOfWeek))
  
  +            Option(featuresWithMetadata.userRequestDowAggregates.get(dayOfWeek))
  
                 .map(featuresWithMetadata.toDataRecord)
  
   
  
             dropUnknownFeatures(userAggregatesDr, userAggregateFeatureInfo.featureContext)
  
