a/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/tweetypie/RequestFields.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/tweetypie/RequestFields.scala
==================================================

Change Range: -44,7 +44,8

Content:

::

index 9d048005e..e20aead94 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/tweetypie/RequestFields.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/tweetypie/RequestFields.scala
  
         tp.TweetInclude.TweetFieldId(tp.Tweet.QuotedTweetField.id),
  
         tp.TweetInclude.TweetFieldId(tp.Tweet.CommunitiesField.id),
  
         // Field required for determining if a Tweet was created via News Camera.
  
  -      tp.TweetInclude.TweetFieldId(tp.Tweet.ComposerSourceField.id)
  
  +      tp.TweetInclude.TweetFieldId(tp.Tweet.ComposerSourceField.id),
  
  +      tp.TweetInclude.TweetFieldId(tp.Tweet.LanguageField.id)
  
       )
  
   
  
     val TweetStaticEntitiesFields: Set[tp.TweetInclude] =
  
