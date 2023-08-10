a/home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/request/HomeMixerProductContextUnmarshaller.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/request/HomeMixerProductContextUnmarshaller.scala
==================================================

Change Range: -46,7 +48,13

Content:

::

index bbc93389c..ec3a183b9 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/request/HomeMixerProductContextUnmarshaller.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/request/HomeMixerProductContextUnmarshaller.scala
  
   import com.twitter.home_mixer.model.request.ListRecommendedUsersProductContext
  
   import com.twitter.home_mixer.model.request.ListTweetsProductContext
  
   import com.twitter.home_mixer.model.request.ScoredTweetsProductContext
  
  +import com.twitter.home_mixer.model.request.SubscribedProductContext
  
   import com.twitter.home_mixer.{thriftscala => t}
  
   import com.twitter.product_mixer.core.model.marshalling.request.ProductContext
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
         ForYouProductContext(
  
           deviceContext = p.deviceContext.map(deviceContextUnmarshaller(_)),
  
           seenTweetIds = p.seenTweetIds,
  
  -        dspClientContext = p.dspClientContext
  
  +        dspClientContext = p.dspClientContext,
  
  +        pushToHomeTweetId = p.pushToHomeTweetId
  
         )
  
  -    case t.ProductContext.Realtime(p) =>
  
  +    case t.ProductContext.ListManagement(p) =>
  
         throw new UnsupportedOperationException(s"This product is no longer used")
  
       case t.ProductContext.ScoredTweets(p) =>
  
         ScoredTweetsProductContext(
  
           deviceContext = p.deviceContext.map(deviceContextUnmarshaller(_)),
  
           seenTweetIds = p.seenTweetIds,
  
  -        servedTweetIds = p.servedTweetIds
  
  +        servedTweetIds = p.servedTweetIds,
  
  +        backfillTweetIds = p.backfillTweetIds
  
         )
  
       case t.ProductContext.ListTweets(p) =>
  
         ListTweetsProductContext(
  
         ListRecommendedUsersProductContext(
  
           listId = p.listId,
  
           selectedUserIds = p.selectedUserIds,
  
  -        excludedUserIds = p.excludedUserIds
  
  +        excludedUserIds = p.excludedUserIds,
  
  +        listName = p.listName
  
  +      )
  
  +    case t.ProductContext.Subscribed(p) =>
  
  +      SubscribedProductContext(
  
  +        deviceContext = p.deviceContext.map(deviceContextUnmarshaller(_)),
  
  +        seenTweetIds = p.seenTweetIds,
  
         )
  
       case t.ProductContext.UnknownUnionField(field) =>
  
         throw new UnsupportedOperationException(s"Unknown display context: ${field.field.name}")
  
