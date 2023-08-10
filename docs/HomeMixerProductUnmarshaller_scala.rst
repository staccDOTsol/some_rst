a/home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/request/HomeMixerProductUnmarshaller.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/request/HomeMixerProductUnmarshaller.scala
==================================================

Change Range: -17,11 +17,12

Content:

::

index 0089c5efb..f5d0d002b 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/request/HomeMixerProductUnmarshaller.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/request/HomeMixerProductUnmarshaller.scala
  
   import com.twitter.home_mixer.model.request.ListRecommendedUsersProduct
  
   import com.twitter.home_mixer.model.request.ListTweetsProduct
  
   import com.twitter.home_mixer.model.request.ScoredTweetsProduct
  
  +import com.twitter.home_mixer.model.request.SubscribedProduct
  
   import com.twitter.home_mixer.{thriftscala => t}
  
   import com.twitter.product_mixer.core.model.marshalling.request.Product
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
     def apply(product: t.Product): Product = product match {
  
       case t.Product.Following => FollowingProduct
  
       case t.Product.ForYou => ForYouProduct
  
  -    case t.Product.Realtime =>
  
  +    case t.Product.ListManagement =>
  
         throw new UnsupportedOperationException(s"This product is no longer used")
  
       case t.Product.ScoredTweets => ScoredTweetsProduct
  
       case t.Product.ListTweets => ListTweetsProduct
  
       case t.Product.ListRecommendedUsers => ListRecommendedUsersProduct
  
  +    case t.Product.Subscribed => SubscribedProduct
  
       case t.Product.EnumUnknownProduct(value) =>
  
         throw new UnsupportedOperationException(s"Unknown product: $value")
  
     }
  
