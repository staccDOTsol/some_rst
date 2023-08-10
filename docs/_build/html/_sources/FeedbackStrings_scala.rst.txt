a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/FeedbackStrings.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/FeedbackStrings.scala
==================================================

Change Range: -0,0 +1,18

Content:

::

new file mode 100644
  
  index 000000000..8a9c214aa
  
  --- /dev/null
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/FeedbackStrings.scala
  
  +package com.twitter.home_mixer.functional_component.decorator.urt.builder
  
  +
  
  +import com.twitter.product_mixer.core.product.guice.scope.ProductScoped
  
  +import com.twitter.stringcenter.client.ExternalStringRegistry
  
  +import javax.inject.Inject
  
  +import javax.inject.Provider
  
  +import javax.inject.Singleton
  
  +
  
  +@Singleton
  
  +class FeedbackStrings @Inject() (
  
  +  @ProductScoped externalStringRegistryProvider: Provider[ExternalStringRegistry]) {
  
  +  private val externalStringRegistry = externalStringRegistryProvider.get()
  
  +
  
  +  val seeLessOftenFeedbackString =
  
  +    externalStringRegistry.createProdString("Feedback.seeLessOften")
  
  +  val seeLessOftenConfirmationFeedbackString =
  
  +    externalStringRegistry.createProdString("Feedback.seeLessOftenConfirmation")
  
  +}
  
