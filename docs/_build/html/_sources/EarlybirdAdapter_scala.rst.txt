a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/adapters/earlybird/EarlybirdAdapter.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/adapters/earlybird/EarlybirdAdapter.scala
==================================================

Change Range: -305,6 +305,10

Content:

::

similarity index 98%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/adapters/earlybird/EarlybirdAdapter.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/adapters/earlybird/EarlybirdAdapter.scala
  
  index 833733c16..5a0207e8a 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/adapters/earlybird/EarlybirdAdapter.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/adapters/earlybird/EarlybirdAdapter.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator.adapters.earlybird
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator.adapters.earlybird
  
   
  
   import com.twitter.ml.api.Feature
  
   import com.twitter.ml.api.FeatureContext
  
         richDataRecord.setFeatureValueFromOption(
  
           RecapFeatures.REPLY_COUNT_V2,
  
           features.replyCountV2.map(_.toDouble))
  
  +      richDataRecord.setFeatureValueFromOption(
  
  +        RecapFeatures.MENTIONED_SCREEN_NAMES,
  
  +        features.mentionsList.map(_.toSet.asJava)
  
  +      )
  
         val urls = features.urlsList.getOrElse(Seq.empty)
  
         richDataRecord.setFeatureValue(
  
           RecapFeatures.URL_DOMAINS,
  
