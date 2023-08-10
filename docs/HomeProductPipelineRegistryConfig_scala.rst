a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/HomeProductPipelineRegistryConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/HomeProductPipelineRegistryConfig.scala
==================================================

Change Range: -44,11 +45,16

Content:

::

index 213143351..5db4bd7f6 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/HomeProductPipelineRegistryConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/HomeProductPipelineRegistryConfig.scala
  
   import com.twitter.home_mixer.model.request.ListRecommendedUsersProduct
  
   import com.twitter.home_mixer.model.request.ListTweetsProduct
  
   import com.twitter.home_mixer.model.request.ScoredTweetsProduct
  
  +import com.twitter.home_mixer.model.request.SubscribedProduct
  
   import com.twitter.home_mixer.product.following.FollowingProductPipelineConfig
  
   import com.twitter.home_mixer.product.for_you.ForYouProductPipelineConfig
  
   import com.twitter.home_mixer.product.list_recommended_users.ListRecommendedUsersProductPipelineConfig
  
   import com.twitter.home_mixer.product.scored_tweets.ScoredTweetsProductPipelineConfig
  
   import com.twitter.home_mixer.product.list_tweets.ListTweetsProductPipelineConfig
  
  +import com.twitter.home_mixer.product.subscribed.SubscribedProductPipelineConfig
  
   import com.twitter.inject.Injector
  
   import com.twitter.product_mixer.core.product.guice.ProductScope
  
   import com.twitter.product_mixer.core.product.registry.ProductPipelineRegistryConfig
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
         injector.instance[ListRecommendedUsersProductPipelineConfig]
  
       }
  
   
  
  +  private val subscribedProductPipelineConfig = productScope.let(SubscribedProduct) {
  
  +    injector.instance[SubscribedProductPipelineConfig]
  
  +  }
  
  +
  
     override val productPipelineConfigs = Seq(
  
       followingProductPipelineConfig,
  
       forYouProductPipelineConfig,
  
       scoredTweetsProductPipelineConfig,
  
       listTweetsProductPipelineConfig,
  
  -    listRecommendedUsersProductPipelineConfig
  
  +    listRecommendedUsersProductPipelineConfig,
  
  +    subscribedProductPipelineConfig,
  
     )
  
   }
  
