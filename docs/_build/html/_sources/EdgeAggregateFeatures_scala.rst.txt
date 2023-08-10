a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/offline_aggregates/EdgeAggregateFeatures.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/offline_aggregates/EdgeAggregateFeatures.scala
==================================================

Change Range: -57,13 +56,25

Content:

::

similarity index 79%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/offline_aggregates/EdgeAggregateFeatures.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/offline_aggregates/EdgeAggregateFeatures.scala
  
  index d637b3b3d..f769bb980 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/offline_aggregates/EdgeAggregateFeatures.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/offline_aggregates/EdgeAggregateFeatures.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator.offline_aggregates
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator.offline_aggregates
  
   
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.TSPInferredTopicFeature
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.adapters.offline_aggregates.PassThroughAdapter
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.adapters.offline_aggregates.SparseAggregatesToDenseAdapter
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.adapters.offline_aggregates.PassThroughAdapter
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.adapters.offline_aggregates.SparseAggregatesToDenseAdapter
  
   import com.twitter.home_mixer.model.HomeFeatures.AuthorIdFeature
  
  -import com.twitter.home_mixer.model.HomeFeatures.MentionUserIdFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.MentionScreenNameFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.TopicIdSocialContextFeature
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.TSPInferredTopicFeature
  
   import com.twitter.home_mixer.util.CandidatesUtil
  
   import com.twitter.timelines.data_processing.ml_util.aggregation_framework.AggregateType
  
   import com.twitter.timelines.prediction.common.aggregates.TimelinesAggregationConfig
  
   
  
     object UserOriginalAuthorAggregateFeature
  
         extends BaseEdgeAggregateFeature(
  
  -        aggregateGroups = Set(
  
  -          TimelinesAggregationConfig.userOriginalAuthorReciprocalEngagementAggregates),
  
  +        aggregateGroups = Set(TimelinesAggregationConfig.userOriginalAuthorAggregatesV1),
  
           aggregateType = AggregateType.UserOriginalAuthor,
  
           extractMapFn = _.userOriginalAuthorAggregates,
  
           adapter = PassThroughAdapter,
  
           extractMapFn = _.userMentionAggregates,
  
           adapter = new SparseAggregatesToDenseAdapter(CombineCountPolicies.MentionCountsPolicy),
  
           getSecondaryKeysFn = candidate =>
  
  -          candidate.features.getOrElse(MentionUserIdFeature, Seq.empty)
  
  +          candidate.features.getOrElse(MentionScreenNameFeature, Seq.empty).map(_.hashCode.toLong)
  
         )
  
   
  
     object UserInferredTopicAggregateFeature
  
         extends BaseEdgeAggregateFeature(
  
           aggregateGroups = Set(
  
             TimelinesAggregationConfig.userInferredTopicAggregates,
  
  +        ),
  
  +        aggregateType = AggregateType.UserInferredTopic,
  
  +        extractMapFn = _.userInferredTopicAggregates,
  
  +        adapter = new SparseAggregatesToDenseAdapter(
  
  +          CombineCountPolicies.UserInferredTopicCountsPolicy),
  
  +        getSecondaryKeysFn = candidate =>
  
  +          candidate.features.getOrElse(TSPInferredTopicFeature, Map.empty[Long, Double]).keys.toSeq
  
  +      )
  
  +
  
  +  object UserInferredTopicAggregateV2Feature
  
  +      extends BaseEdgeAggregateFeature(
  
  +        aggregateGroups = Set(
  
             TimelinesAggregationConfig.userInferredTopicAggregatesV2
  
           ),
  
           aggregateType = AggregateType.UserInferredTopic,
  
