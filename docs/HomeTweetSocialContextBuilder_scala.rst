a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/HomeTweetSocialContextBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/HomeTweetSocialContextBuilder.scala
==================================================

Change Range: -31,6 +34,9

Content:

::

similarity index 80%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/HomeTweetSocialContextBuilder.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/HomeTweetSocialContextBuilder.scala
  
  index 4f3e03f6d..5138f254a 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/HomeTweetSocialContextBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/HomeTweetSocialContextBuilder.scala
  
  -package com.twitter.home_mixer.functional_component.decorator
  
  +package com.twitter.home_mixer.functional_component.decorator.urt.builder
  
   
  
   import com.twitter.home_mixer.model.HomeFeatures.ConversationModuleFocalTweetIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.ConversationModuleIdFeature
  
   @Singleton
  
   case class HomeTweetSocialContextBuilder @Inject() (
  
     likedBySocialContextBuilder: LikedBySocialContextBuilder,
  
  +  listsSocialContextBuilder: ListsSocialContextBuilder,
  
     followedBySocialContextBuilder: FollowedBySocialContextBuilder,
  
     topicSocialContextBuilder: TopicSocialContextBuilder,
  
     extendedReplySocialContextBuilder: ExtendedReplySocialContextBuilder,
  
  -  receivedReplySocialContextBuilder: ReceivedReplySocialContextBuilder)
  
  +  receivedReplySocialContextBuilder: ReceivedReplySocialContextBuilder,
  
  +  popularVideoSocialContextBuilder: PopularVideoSocialContextBuilder,
  
  +  popularInYourAreaSocialContextBuilder: PopularInYourAreaSocialContextBuilder)
  
       extends BaseSocialContextBuilder[PipelineQuery, TweetCandidate] {
  
   
  
     def apply(
  
             likedBySocialContextBuilder(query, candidate, features)
  
               .orElse(followedBySocialContextBuilder(query, candidate, features))
  
               .orElse(topicSocialContextBuilder(query, candidate, features))
  
  +            .orElse(popularVideoSocialContextBuilder(query, candidate, features))
  
  +            .orElse(listsSocialContextBuilder(query, candidate, features))
  
  +            .orElse(popularInYourAreaSocialContextBuilder(query, candidate, features))
  
           case Some(_) =>
  
             val conversationId = features.getOrElse(ConversationModuleIdFeature, None)
  
             // Only hydrate the social context into the root tweet in a conversation module
  
