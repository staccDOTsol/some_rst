a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/PopularInYourAreaSocialContextBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/PopularInYourAreaSocialContextBuilder.scala
==================================================

Change Range: -0,0 +1,43

Content:

::

new file mode 100644
  
  index 000000000..3782dd115
  
  --- /dev/null
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/PopularInYourAreaSocialContextBuilder.scala
  
  +package com.twitter.home_mixer.functional_component.decorator.urt.builder
  
  +
  
  +import com.twitter.home_mixer.model.HomeFeatures.SuggestTypeFeature
  
  +import com.twitter.home_mixer.product.following.model.HomeMixerExternalStrings
  
  +import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
  +import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
  +import com.twitter.product_mixer.core.functional_component.decorator.urt.builder.social_context.BaseSocialContextBuilder
  
  +import com.twitter.product_mixer.core.model.marshalling.response.urt.metadata._
  
  +import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  +import com.twitter.product_mixer.core.product.guice.scope.ProductScoped
  
  +import com.twitter.stringcenter.client.StringCenter
  
  +import com.twitter.timelineservice.suggests.{thriftscala => st}
  
  +import javax.inject.Inject
  
  +import javax.inject.Provider
  
  +import javax.inject.Singleton
  
  +
  
  +@Singleton
  
  +case class PopularInYourAreaSocialContextBuilder @Inject() (
  
  +  externalStrings: HomeMixerExternalStrings,
  
  +  @ProductScoped stringCenterProvider: Provider[StringCenter])
  
  +    extends BaseSocialContextBuilder[PipelineQuery, TweetCandidate] {
  
  +
  
  +  private val stringCenter = stringCenterProvider.get()
  
  +  private val popularInYourAreaString = externalStrings.socialContextPopularInYourAreaString
  
  +
  
  +  def apply(
  
  +    query: PipelineQuery,
  
  +    candidate: TweetCandidate,
  
  +    candidateFeatures: FeatureMap
  
  +  ): Option[SocialContext] = {
  
  +    val suggestTypeOpt = candidateFeatures.getOrElse(SuggestTypeFeature, None)
  
  +    if (suggestTypeOpt.contains(st.SuggestType.RecommendedTrendTweet)) {
  
  +      Some(
  
  +        GeneralContext(
  
  +          contextType = LocationGeneralContextType,
  
  +          text = stringCenter.prepare(popularInYourAreaString),
  
  +          url = None,
  
  +          contextImageUrls = None,
  
  +          landingUrl = None
  
  +        ))
  
  +    } else None
  
  +  }
  
  +}
  
