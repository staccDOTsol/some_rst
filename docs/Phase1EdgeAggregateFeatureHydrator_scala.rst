a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/offline_aggregates/Phase1EdgeAggregateFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/offline_aggregates/Phase1EdgeAggregateFeatureHydrator.scala
==================================================

Change Range: -11,10 +11,9

Content:

::

similarity index 56%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/offline_aggregates/Phase1EdgeAggregateFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/offline_aggregates/Phase1EdgeAggregateFeatureHydrator.scala
  
  index c8c912b63..d9df51d5c 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/offline_aggregates/Phase1EdgeAggregateFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/offline_aggregates/Phase1EdgeAggregateFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator.offline_aggregates
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator.offline_aggregates
  
   
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.offline_aggregates.EdgeAggregateFeatures._
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.offline_aggregates.EdgeAggregateFeatures._
  
   import com.twitter.product_mixer.core.model.common.identifier.FeatureHydratorIdentifier
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
     override val identifier: FeatureHydratorIdentifier =
  
       FeatureHydratorIdentifier("Phase1EdgeAggregate")
  
   
  
  -  override val aggregateFeatures: Set[BaseEdgeAggregateFeature] =
  
  -    Set(
  
  -      UserAuthorAggregateFeature,
  
  -      UserOriginalAuthorAggregateFeature,
  
  -      UserMentionAggregateFeature
  
  -    )
  
  +  override val aggregateFeatures: Set[BaseEdgeAggregateFeature] = Set(
  
  +    UserAuthorAggregateFeature,
  
  +    UserOriginalAuthorAggregateFeature,
  
  +    UserMentionAggregateFeature
  
  +  )
  
   }
  
