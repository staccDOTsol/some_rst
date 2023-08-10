a/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/HomeGlobalParams.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/HomeGlobalParams.scala
==================================================

Change Range: -52,59 +52,35

Content:

::

index 0bd5dd1f0..f19817bc9 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/HomeGlobalParams.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/HomeGlobalParams.scala
  
           default = true
  
         )
  
   
  
  -  object EnableServedCandidateKafkaPublishingParam
  
  -      extends FSParam[Boolean](
  
  -        name = "home_mixer_enable_served_candidate_kafka_publishing",
  
  -        default = true
  
  -      )
  
  -
  
  -  /**
  
  -   * This author ID list is used purely for realtime metrics collection around how often we
  
  -   * are serving Tweets from these authors and which candidate sources they are coming from.
  
  -   */
  
  -  object AuthorListForStatsParam
  
  -      extends FSParam[Set[Long]](
  
  -        name = "home_mixer_author_list_for_stats",
  
  -        default = Set.empty
  
  -      )
  
  -
  
     object EnableSocialContextParam
  
         extends FSParam[Boolean](
  
           name = "home_mixer_enable_social_context",
  
  -        default = false
  
  -      )
  
  -
  
  -  object EnableGizmoduckAuthorSafetyFeatureHydratorParam
  
  -      extends FSParam[Boolean](
  
  -        name = "home_mixer_enable_gizmoduck_author_safety_feature_hydrator",
  
           default = true
  
         )
  
   
  
  -  object EnableFeedbackFatigueParam
  
  +  object EnableAdvertiserBrandSafetySettingsFeatureHydratorParam
  
         extends FSParam[Boolean](
  
  -        name = "home_mixer_enable_feedback_fatigue",
  
  +        name = "home_mixer_enable_advertiser_brand_safety_settings_feature_hydrator",
  
           default = true
  
         )
  
   
  
  -  object BlueVerifiedAuthorInNetworkMultiplierParam
  
  -      extends FSBoundedParam[Double](
  
  -        name = "home_mixer_blue_verified_author_in_network_multiplier",
  
  -        default = 4.0,
  
  -        min = 0.0,
  
  -        max = 100.0
  
  +  object EnableImpressionBloomFilter
  
  +      extends FSParam[Boolean](
  
  +        name = "home_mixer_enable_impression_bloom_filter",
  
  +        default = false
  
         )
  
   
  
  -  object BlueVerifiedAuthorOutOfNetworkMultiplierParam
  
  +  object ImpressionBloomFilterFalsePositiveRateParam
  
         extends FSBoundedParam[Double](
  
  -        name = "home_mixer_blue_verified_author_out_of_network_multiplier",
  
  -        default = 2.0,
  
  -        min = 0.0,
  
  -        max = 100.0
  
  +        name = "home_mixer_impression_bloom_filter_false_positive_rate",
  
  +        default = 0.005,
  
  +        min = 0.001,
  
  +        max = 0.01
  
         )
  
   
  
  -  object EnableAdvertiserBrandSafetySettingsFeatureHydratorParam
  
  +  object EnableScribeServedCandidatesParam
  
         extends FSParam[Boolean](
  
  -        name = "home_mixer_enable_advertiser_brand_safety_settings_feature_hydrator",
  
  +        name = "home_mixer_served_tweets_enable_scribing",
  
           default = true
  
         )
  
   }
  
