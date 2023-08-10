a/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/HomeGlobalParamConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/HomeGlobalParamConfig.scala
==================================================

Change Range: -30,11 +29,6

Content:

::

index a998aa9ca..6fbd28fce 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/HomeGlobalParamConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/HomeGlobalParamConfig.scala
  
   
  
     override val booleanFSOverrides = Seq(
  
       AdsDisableInjectionBasedOnUserRoleParam,
  
  -    EnableSendScoresToClient,
  
  +    EnableAdvertiserBrandSafetySettingsFeatureHydratorParam,
  
  +    EnableImpressionBloomFilter,
  
       EnableNahFeedbackInfoParam,
  
       EnableNewTweetsPillAvatarsParam,
  
  -    EnableServedCandidateKafkaPublishingParam,
  
  +    EnableScribeServedCandidatesParam,
  
  +    EnableSendScoresToClient,
  
       EnableSocialContextParam,
  
  -    EnableGizmoduckAuthorSafetyFeatureHydratorParam,
  
  -    EnableAdvertiserBrandSafetySettingsFeatureHydratorParam,
  
  -    EnableFeedbackFatigueParam
  
     )
  
   
  
     override val boundedIntFSOverrides = Seq(
  
     )
  
   
  
     override val boundedDoubleFSOverrides = Seq(
  
  -    BlueVerifiedAuthorInNetworkMultiplierParam,
  
  -    BlueVerifiedAuthorOutOfNetworkMultiplierParam
  
  -  )
  
  -
  
  -  override val longSetFSOverrides = Seq(
  
  -    AuthorListForStatsParam
  
  +    ImpressionBloomFilterFalsePositiveRateParam
  
     )
  
   }
  
