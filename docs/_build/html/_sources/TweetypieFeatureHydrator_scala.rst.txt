a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TweetypieFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TweetypieFeatureHydrator.scala
==================================================

Change Range: -137,20 +156,24

Content:

::

index 09fc62405..f2effa1b2 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TweetypieFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TweetypieFeatureHydrator.scala
  
   package com.twitter.home_mixer.functional_component.feature_hydrator
  
   
  
  +import com.twitter.finagle.stats.StatsReceiver
  
   import com.twitter.home_mixer.model.HomeFeatures.AuthorIdFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.ExclusiveConversationAuthorIdFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.InNetworkFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.InReplyToTweetIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.IsHydratedFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.IsNsfwFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.QuotedUserIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.SourceTweetIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.SourceUserIdFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.TweetLanguageFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.TweetTextFeature
  
   import com.twitter.home_mixer.model.request.FollowingProduct
  
   import com.twitter.home_mixer.model.request.ForYouProduct
  
   import com.twitter.home_mixer.model.request.ListTweetsProduct
  
   import com.twitter.home_mixer.model.request.ScoredTweetsProduct
  
  +import com.twitter.home_mixer.model.request.SubscribedProduct
  
   import com.twitter.home_mixer.util.tweetypie.RequestFields
  
   import com.twitter.product_mixer.component_library.feature_hydrator.candidate.tweet_is_nsfw.IsNsfw
  
   import com.twitter.product_mixer.component_library.feature_hydrator.candidate.tweet_visibility_reason.VisibilityReason
  
   import com.twitter.stitch.Stitch
  
   import com.twitter.stitch.tweetypie.{TweetyPie => TweetypieStitchClient}
  
   import com.twitter.tweetypie.{thriftscala => tp}
  
  +import com.twitter.util.logging.Logging
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
   @Singleton
  
  -class TweetypieFeatureHydrator @Inject() (tweetypieStitchClient: TweetypieStitchClient)
  
  -    extends CandidateFeatureHydrator[PipelineQuery, TweetCandidate] {
  
  +class TweetypieFeatureHydrator @Inject() (
  
  +  tweetypieStitchClient: TweetypieStitchClient,
  
  +  statsReceiver: StatsReceiver)
  
  +    extends CandidateFeatureHydrator[PipelineQuery, TweetCandidate]
  
  +    with Logging {
  
   
  
     override val identifier: FeatureHydratorIdentifier = FeatureHydratorIdentifier("Tweetypie")
  
   
  
     override val features: Set[Feature[_, _]] = Set(
  
       AuthorIdFeature,
  
  +    ExclusiveConversationAuthorIdFeature,
  
       InReplyToTweetIdFeature,
  
       IsHydratedFeature,
  
       IsNsfw,
  
       SourceTweetIdFeature,
  
       SourceUserIdFeature,
  
       TweetTextFeature,
  
  +    TweetLanguageFeature,
  
       VisibilityReason
  
     )
  
   
  
     ): Stitch[FeatureMap] = {
  
       val safetyLevel = query.product match {
  
         case FollowingProduct => rtf.SafetyLevel.TimelineHomeLatest
  
  -      case ForYouProduct => rtf.SafetyLevel.TimelineHome
  
  +      case ForYouProduct =>
  
  +        val inNetwork = existingFeatures.getOrElse(InNetworkFeature, true)
  
  +        if (inNetwork) rtf.SafetyLevel.TimelineHome else rtf.SafetyLevel.TimelineHomeRecommendations
  
         case ScoredTweetsProduct => rtf.SafetyLevel.TimelineHome
  
         case ListTweetsProduct => rtf.SafetyLevel.TimelineLists
  
  +      case SubscribedProduct => rtf.SafetyLevel.TimelineHomeSubscribed
  
         case unknown => throw new UnsupportedOperationException(s"Unknown product: $unknown")
  
       }
  
   
  
         includeQuotedTweet = true,
  
         visibilityPolicy = tp.TweetVisibilityPolicy.UserVisible,
  
         safetyLevel = Some(safetyLevel),
  
  -      forUserId = Some(query.getRequiredUserId)
  
  +      forUserId = query.getOptionalUserId
  
       )
  
   
  
  +    val exclusiveAuthorIdOpt =
  
  +      existingFeatures.getOrElse(ExclusiveConversationAuthorIdFeature, None)
  
  +
  
       tweetypieStitchClient.getTweetFields(tweetId = candidate.id, options = tweetFieldsOptions).map {
  
         case tp.GetTweetFieldsResult(_, tp.TweetFieldsResultState.Found(found), quoteOpt, _) =>
  
           val coreData = found.tweet.coreData
  
             found.retweetedTweet.exists(_.coreData.exists(data => data.nsfwAdmin || data.nsfwUser))
  
   
  
           val tweetText = coreData.map(_.text)
  
  +        val tweetLanguage = found.tweet.language.map(_.language)
  
   
  
           val tweetAuthorId = coreData.map(_.userId)
  
           val inReplyToTweetId = coreData.flatMap(_.reply.flatMap(_.inReplyToStatusId))
  
   
  
           FeatureMapBuilder()
  
             .add(AuthorIdFeature, tweetAuthorId)
  
  +          .add(ExclusiveConversationAuthorIdFeature, exclusiveAuthorIdOpt)
  
             .add(InReplyToTweetIdFeature, inReplyToTweetId)
  
             .add(IsHydratedFeature, true)
  
             .add(IsNsfw, Some(isNsfw))
  
             .add(QuotedUserIdFeature, quotedTweetUserId)
  
             .add(SourceTweetIdFeature, retweetedTweetId)
  
             .add(SourceUserIdFeature, retweetedTweetUserId)
  
  +          .add(TweetLanguageFeature, tweetLanguage)
  
             .add(TweetTextFeature, tweetText)
  
             .add(VisibilityReason, found.suppressReason)
  
             .build()
  
   
  
         // If no tweet result found, return default and pre-existing features
  
         case _ =>
  
  -        DefaultFeatureMap +
  
  -          (AuthorIdFeature, existingFeatures.getOrElse(AuthorIdFeature, None)) +
  
  -          (InReplyToTweetIdFeature, existingFeatures.getOrElse(InReplyToTweetIdFeature, None)) +
  
  -          (IsRetweetFeature, existingFeatures.getOrElse(IsRetweetFeature, false)) +
  
  -          (QuotedTweetIdFeature, existingFeatures.getOrElse(QuotedTweetIdFeature, None)) +
  
  -          (QuotedUserIdFeature, existingFeatures.getOrElse(QuotedUserIdFeature, None)) +
  
  -          (SourceTweetIdFeature, existingFeatures.getOrElse(SourceTweetIdFeature, None)) +
  
  -          (SourceUserIdFeature, existingFeatures.getOrElse(SourceUserIdFeature, None))
  
  +        DefaultFeatureMap ++ FeatureMapBuilder()
  
  +          .add(AuthorIdFeature, existingFeatures.getOrElse(AuthorIdFeature, None))
  
  +          .add(ExclusiveConversationAuthorIdFeature, exclusiveAuthorIdOpt)
  
  +          .add(InReplyToTweetIdFeature, existingFeatures.getOrElse(InReplyToTweetIdFeature, None))
  
  +          .add(IsRetweetFeature, existingFeatures.getOrElse(IsRetweetFeature, false))
  
  +          .add(QuotedTweetIdFeature, existingFeatures.getOrElse(QuotedTweetIdFeature, None))
  
  +          .add(QuotedUserIdFeature, existingFeatures.getOrElse(QuotedUserIdFeature, None))
  
  +          .add(SourceTweetIdFeature, existingFeatures.getOrElse(SourceTweetIdFeature, None))
  
  +          .add(SourceUserIdFeature, existingFeatures.getOrElse(SourceUserIdFeature, None))
  
  +          .add(TweetLanguageFeature, existingFeatures.getOrElse(TweetLanguageFeature, None))
  
  +          .build()
  
       }
  
     }
  
   }
  
