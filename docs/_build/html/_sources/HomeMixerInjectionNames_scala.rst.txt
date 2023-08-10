a/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/HomeMixerInjectionNames.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/HomeMixerInjectionNames.scala
==================================================

Change Range: -32,18 +32,15

Content:

::

index 739c48a77..5ea87f5ab 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/HomeMixerInjectionNames.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/HomeMixerInjectionNames.scala
  
     final val TweetEngagementCache = "TweetEngagementCache"
  
     final val TweetypieContentRepository = "TweetypieContentRepository"
  
     final val TweetypieStaticEntitiesCache = "TweetypieStaticEntitiesCache"
  
  -  final val TwhinAuthorFollow20200101FeatureCacheClient =
  
  -    "TwhinAuthorFollow20200101FeatureCacheClient"
  
  -  final val TwhinAuthorFollow20200101FeatureRepository =
  
  -    "TwhinAuthorFollow20200101FeatureRepository"
  
  +  final val TwhinAuthorFollowFeatureCacheClient = "TwhinAuthorFollowFeatureCacheClient"
  
  +  final val TwhinAuthorFollowFeatureRepository = "TwhinAuthorFollowFeatureRepository"
  
     final val TwhinUserEngagementFeatureRepository = "TwhinUserEngagementFeatureRepository"
  
     final val TwhinUserFollowFeatureRepository = "TwhinUserFollowFeatureRepository"
  
     final val TwitterListEngagementCache = "TwitterListEngagementCache"
  
     final val UserAuthorEngagementCache = "UserAuthorEngagementCache"
  
     final val UserEngagementCache = "UserEngagementCache"
  
     final val UserFollowedTopicIdsRepository = "UserFollowedTopicIdsRepository"
  
  -  final val UserLanguagesStore = "UserLanguagesStore"
  
  -  final val UserMetadataManhattanEndpoint = "UserMetadataManhattanEndpoint"
  
  +  final val UserLanguagesRepository = "UserLanguagesRepository"
  
     final val UserTopicEngagementForNewUserCache = "UserTopicEngagementForNewUserCache"
  
     final val UtegSocialProofRepository = "UtegSocialProofRepository"
  
   }
  
