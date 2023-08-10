a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/adapters/content/ContentFeatureAdapter.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/adapters/content/ContentFeatureAdapter.scala
==================================================

Change Range: -79,6 +82,16

Content:

::

similarity index 94%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/adapters/content/ContentFeatureAdapter.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/adapters/content/ContentFeatureAdapter.scala
  
  index 93cb6036d..2d73ece45 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/adapters/content/ContentFeatureAdapter.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/adapters/content/ContentFeatureAdapter.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator.adapters.content
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator.adapters.content
  
   
  
   import com.twitter.home_mixer.model.ContentFeatures
  
   import com.twitter.ml.api.Feature
  
   import com.twitter.timelines.prediction.common.adapters.TimelinesMutatingAdapterBase
  
   import com.twitter.timelines.prediction.common.adapters.TweetLengthType
  
   import com.twitter.timelines.prediction.features.common.TimelinesSharedFeatures
  
  +import com.twitter.timelines.prediction.features.conversation_features.ConversationFeatures
  
   import scala.collection.JavaConverters._
  
   
  
   object ContentFeatureAdapter extends TimelinesMutatingAdapterBase[Option[ContentFeatures]] {
  
   
  
     override val getFeatureContext: FeatureContext = new FeatureContext(
  
  +    ConversationFeatures.IS_SELF_THREAD_TWEET,
  
  +    ConversationFeatures.IS_LEAF_IN_SELF_THREAD,
  
       TimelinesSharedFeatures.ASPECT_RATIO_DEN,
  
       TimelinesSharedFeatures.ASPECT_RATIO_NUM,
  
       TimelinesSharedFeatures.BIT_RATE,
  
     ): Unit = {
  
       if (contentFeatures.nonEmpty) {
  
         val features = contentFeatures.get
  
  +      // Conversation Features
  
  +      richDataRecord.setFeatureValueFromOption(
  
  +        ConversationFeatures.IS_SELF_THREAD_TWEET,
  
  +        Some(features.selfThreadMetadata.nonEmpty)
  
  +      )
  
  +      richDataRecord.setFeatureValueFromOption(
  
  +        ConversationFeatures.IS_LEAF_IN_SELF_THREAD,
  
  +        features.selfThreadMetadata.map(_.isLeaf)
  
  +      )
  
  +
  
         // Media Features
  
         richDataRecord.setFeatureValueFromOption(
  
           TimelinesSharedFeatures.ASPECT_RATIO_DEN,
  
