a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RetweetSourceTweetFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/RetweetSourceTweetFeatureHydrator.scala
==================================================

Change Range: -22,8 +22,8

Content:

::

similarity index 94%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RetweetSourceTweetFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/RetweetSourceTweetFeatureHydrator.scala
  
  index 98385b130..33cbef884 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RetweetSourceTweetFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/RetweetSourceTweetFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator
  
   
  
   import com.twitter.home_mixer.model.HomeFeatures._
  
   import com.twitter.product_mixer.component_library.candidate_source.timeline_ranker.TimelineRankerInNetworkSourceTweetsByTweetIdMapFeature
  
   object RetweetSourceTweetFeatureHydrator
  
       extends BulkCandidateFeatureHydrator[PipelineQuery, TweetCandidate] {
  
   
  
  -  override val identifier: FeatureHydratorIdentifier = FeatureHydratorIdentifier(
  
  -    "RetweetSourceTweet")
  
  +  override val identifier: FeatureHydratorIdentifier =
  
  +    FeatureHydratorIdentifier("RetweetSourceTweet")
  
   
  
     override val features: Set[Feature[_, _]] = Set(
  
       SourceTweetEarlybirdFeature,
  
