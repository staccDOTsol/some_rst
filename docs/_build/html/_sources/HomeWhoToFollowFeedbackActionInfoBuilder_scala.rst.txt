a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/HomeWhoToFollowFeedbackActionInfoBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/HomeWhoToFollowFeedbackActionInfoBuilder.scala
==================================================

Change Range: -32,12 +31,13

Content:

::

index fd108dc49..49017e8cf 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/HomeWhoToFollowFeedbackActionInfoBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/HomeWhoToFollowFeedbackActionInfoBuilder.scala
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
   import com.twitter.product_mixer.core.functional_component.decorator.urt.builder.metadata.BaseFeedbackActionInfoBuilder
  
   import com.twitter.product_mixer.core.product.guice.scope.ProductScoped
  
  -import com.twitter.stringcenter.client.ExternalStringRegistry
  
   import com.twitter.stringcenter.client.StringCenter
  
   import javax.inject.Inject
  
   import javax.inject.Provider
  
   
  
   @Singleton
  
   case class HomeWhoToFollowFeedbackActionInfoBuilder @Inject() (
  
  -  @ProductScoped externalStringRegistryProvider: Provider[ExternalStringRegistry],
  
  +  feedbackStrings: FeedbackStrings,
  
     @ProductScoped stringCenterProvider: Provider[StringCenter])
  
       extends BaseFeedbackActionInfoBuilder[PipelineQuery, UserCandidate] {
  
   
  
     private val whoToFollowFeedbackActionInfoBuilder = WhoToFollowFeedbackActionInfoBuilder(
  
  -    externalStringRegistry = externalStringRegistryProvider.get(),
  
  +    seeLessOftenFeedbackString = feedbackStrings.seeLessOftenFeedbackString,
  
  +    seeLessOftenConfirmationFeedbackString = feedbackStrings.seeLessOftenConfirmationFeedbackString,
  
       stringCenter = stringCenterProvider.get(),
  
       encodedFeedbackRequest = Some(HomeWhoToFollowFeedbackActionInfoBuilder.EncodedFeedbackRequest)
  
     )
  
