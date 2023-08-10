a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/YouMightLikeSocialContextBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/PopularVideoSocialContextBuilder.scala
==================================================

Change Range: -1,50 +1,48

Content:

::

similarity index 60%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/YouMightLikeSocialContextBuilder.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/PopularVideoSocialContextBuilder.scala
  
  index e131a67f0..7eef7f180 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/YouMightLikeSocialContextBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/PopularVideoSocialContextBuilder.scala
  
  -package com.twitter.home_mixer.functional_component.decorator
  
  +package com.twitter.home_mixer.functional_component.decorator.urt.builder
  
   
  
  -import com.twitter.home_mixer.model.HomeFeatures.InNetworkFeature
  
  -import com.twitter.home_mixer.model.HomeFeatures.IsRetweetFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.SuggestTypeFeature
  
   import com.twitter.home_mixer.product.following.model.HomeMixerExternalStrings
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
   import com.twitter.product_mixer.core.functional_component.decorator.urt.builder.social_context.BaseSocialContextBuilder
  
  -import com.twitter.product_mixer.core.model.marshalling.response.urt.metadata.SocialContext
  
   import com.twitter.product_mixer.core.model.marshalling.response.urt.metadata._
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
   import com.twitter.product_mixer.core.product.guice.scope.ProductScoped
  
   import com.twitter.stringcenter.client.StringCenter
  
  +import com.twitter.timelineservice.suggests.{thriftscala => st}
  
   import javax.inject.Inject
  
   import javax.inject.Provider
  
   import javax.inject.Singleton
  
   
  
  -/**
  
  - * Renders a fixed 'You Might Like' string above all OON Tweets.
  
  - */
  
   @Singleton
  
  -case class YouMightLikeSocialContextBuilder @Inject() (
  
  +case class PopularVideoSocialContextBuilder @Inject() (
  
     externalStrings: HomeMixerExternalStrings,
  
     @ProductScoped stringCenterProvider: Provider[StringCenter])
  
       extends BaseSocialContextBuilder[PipelineQuery, TweetCandidate] {
  
   
  
     private val stringCenter = stringCenterProvider.get()
  
  -  private val youMightLikeString = externalStrings.socialContextYouMightLikeString
  
  +  private val popularVideoString = externalStrings.socialContextPopularVideoString
  
   
  
     def apply(
  
       query: PipelineQuery,
  
       candidate: TweetCandidate,
  
       candidateFeatures: FeatureMap
  
     ): Option[SocialContext] = {
  
  -    val isInNetwork = candidateFeatures.getOrElse(InNetworkFeature, true)
  
  -    val isRetweet = candidateFeatures.getOrElse(IsRetweetFeature, false)
  
  -    if (!isInNetwork && !isRetweet) {
  
  +    val suggestTypeOpt = candidateFeatures.getOrElse(SuggestTypeFeature, None)
  
  +    if (suggestTypeOpt.contains(st.SuggestType.MediaTweet)) {
  
         Some(
  
           GeneralContext(
  
             contextType = SparkleGeneralContextType,
  
  -          text = stringCenter.prepare(youMightLikeString),
  
  +          text = stringCenter.prepare(popularVideoString),
  
             url = None,
  
             contextImageUrls = None,
  
  -          landingUrl = None
  
  +          landingUrl = Some(
  
  +            Url(
  
  +              urlType = DeepLink,
  
  +              url = ""
  
  +            )
  
  +          )
  
           ))
  
  -    } else {
  
  -      None
  
  -    }
  
  +    } else None
  
     }
  
   }
  
