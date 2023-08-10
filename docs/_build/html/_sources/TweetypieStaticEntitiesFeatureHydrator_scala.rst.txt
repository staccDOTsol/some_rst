a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TweetypieStaticEntitiesFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/TweetypieStaticEntitiesFeatureHydrator.scala
==================================================

Change Range: -129,7 +130,6

Content:

::

similarity index 94%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TweetypieStaticEntitiesFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/TweetypieStaticEntitiesFeatureHydrator.scala
  
  index 773f2144e..0ea9e8591 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TweetypieStaticEntitiesFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/TweetypieStaticEntitiesFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator
  
   
  
   import com.google.inject.name.Named
  
   import com.twitter.conversions.DurationOps.RichDuration
  
   import com.twitter.home_mixer.model.HomeFeatures.AuthorIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.DirectedAtUserIdFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.ExclusiveConversationAuthorIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.HasImageFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.HasVideoFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.InReplyToTweetIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.MentionUserIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.QuotedTweetIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.QuotedUserIdFeature
  
  -import com.twitter.home_mixer.model.HomeFeatures.SemanticAnnotationFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.SourceTweetIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.SourceUserIdFeature
  
   import com.twitter.home_mixer.param.HomeMixerInjectionNames.TweetypieStaticEntitiesCache
  
     override val features: Set[Feature[_, _]] = Set(
  
       AuthorIdFeature,
  
       DirectedAtUserIdFeature,
  
  +    ExclusiveConversationAuthorIdFeature,
  
       HasImageFeature,
  
       HasVideoFeature,
  
       InReplyToTweetIdFeature,
  
       MentionUserIdFeature,
  
       QuotedTweetIdFeature,
  
       QuotedUserIdFeature,
  
  -    SemanticAnnotationFeature,
  
       SourceTweetIdFeature,
  
       SourceUserIdFeature
  
     )
  
     private val DefaultFeatureMap = FeatureMapBuilder()
  
       .add(AuthorIdFeature, None)
  
       .add(DirectedAtUserIdFeature, None)
  
  +    .add(ExclusiveConversationAuthorIdFeature, None)
  
       .add(HasImageFeature, false)
  
       .add(HasVideoFeature, false)
  
       .add(InReplyToTweetIdFeature, None)
  
       .add(MentionUserIdFeature, Seq.empty)
  
       .add(QuotedTweetIdFeature, None)
  
       .add(QuotedUserIdFeature, None)
  
  -    .add(SemanticAnnotationFeature, Seq.empty)
  
       .add(SourceTweetIdFeature, None)
  
       .add(SourceUserIdFeature, None)
  
       .build()
  
       val mentions = tweet.mentions.getOrElse(Seq.empty)
  
       val share = coreData.flatMap(_.share)
  
       val reply = coreData.flatMap(_.reply)
  
  -    val semanticAnnotations =
  
  -      tweet.escherbirdEntityAnnotations.map(_.entityAnnotations).getOrElse(Seq.empty)
  
   
  
       FeatureMapBuilder()
  
         .add(AuthorIdFeature, coreData.map(_.userId))
  
         .add(DirectedAtUserIdFeature, coreData.flatMap(_.directedAtUser.map(_.userId)))
  
  +      .add(
  
  +        ExclusiveConversationAuthorIdFeature,
  
  +        tweet.exclusiveTweetControl.map(_.conversationAuthorId))
  
         .add(HasImageFeature, TweetMediaFeaturesExtractor.hasImage(tweet))
  
         .add(HasVideoFeature, TweetMediaFeaturesExtractor.hasVideo(tweet))
  
         .add(InReplyToTweetIdFeature, reply.flatMap(_.inReplyToStatusId))
  
         .add(MentionUserIdFeature, mentions.flatMap(_.userId))
  
         .add(QuotedTweetIdFeature, quotedTweet.map(_.tweetId))
  
         .add(QuotedUserIdFeature, quotedTweet.map(_.userId))
  
  -      .add(SemanticAnnotationFeature, semanticAnnotations)
  
         .add(SourceTweetIdFeature, share.map(_.sourceStatusId))
  
         .add(SourceUserIdFeature, share.map(_.sourceUserId))
  
         .build()
  
