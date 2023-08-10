a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/filter/SocialContextFilter.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/filter/ScoredTweetsSocialContextFilter.scala
==================================================

Change Range: -51,7 +54,8

Content:

::

similarity index 65%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/filter/SocialContextFilter.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/filter/ScoredTweetsSocialContextFilter.scala
  
  index 5002137c4..fef427a6d 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/filter/SocialContextFilter.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/filter/ScoredTweetsSocialContextFilter.scala
  
  -package com.twitter.home_mixer.functional_component.filter
  
  -
  
  -import com.twitter.home_mixer.model.HomeFeatures.ConversationModuleFocalTweetIdFeature
  
  -import com.twitter.home_mixer.model.HomeFeatures.InNetworkFeature
  
  -import com.twitter.home_mixer.model.HomeFeatures.PerspectiveFilteredLikedByUserIdsFeature
  
  -import com.twitter.home_mixer.model.HomeFeatures.SGSValidFollowedByUserIdsFeature
  
  -import com.twitter.home_mixer.model.HomeFeatures.SGSValidLikedByUserIdsFeature
  
  -import com.twitter.home_mixer.model.HomeFeatures.TopicContextFunctionalityTypeFeature
  
  -import com.twitter.home_mixer.model.HomeFeatures.TopicIdSocialContextFeature
  
  +package com.twitter.home_mixer.product.scored_tweets.filter
  
  +
  
  +import com.twitter.home_mixer.model.HomeFeatures._
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
   import com.twitter.product_mixer.core.functional_component.filter.Filter
  
   import com.twitter.product_mixer.core.model.common.identifier.FilterIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
   import com.twitter.stitch.Stitch
  
  +import com.twitter.timelineservice.suggests.{thriftscala => st}
  
  +
  
  +object ScoredTweetsSocialContextFilter extends Filter[PipelineQuery, TweetCandidate] {
  
   
  
  -object SocialContextFilter extends Filter[PipelineQuery, TweetCandidate] {
  
  +  override val identifier: FilterIdentifier = FilterIdentifier("ScoredTweetsSocialContext")
  
   
  
  -  override val identifier: FilterIdentifier = FilterIdentifier("SocialContext")
  
  +  // Tweets from candidate sources which don't need generic like/follow/topic proof
  
  +  private val AllowedSources: Set[st.SuggestType] = Set(
  
  +    st.SuggestType.RankedListTweet,
  
  +    st.SuggestType.RecommendedTrendTweet,
  
  +    st.SuggestType.MediaTweet
  
  +  )
  
   
  
     override def apply(
  
       query: PipelineQuery,
  
       val validTweetIds = candidates
  
         .filter { candidate =>
  
           candidate.features.getOrElse(InNetworkFeature, true) ||
  
  +        candidate.features.getOrElse(SuggestTypeFeature, None).exists(AllowedSources.contains) ||
  
  +        candidate.features.getOrElse(InReplyToTweetIdFeature, None).isDefined ||
  
           hasLikedBySocialContext(candidate.features) ||
  
           hasFollowedBySocialContext(candidate.features) ||
  
  -        hasTopicSocialContext(candidate.features) ||
  
  -        candidate.features.getOrElse(ConversationModuleFocalTweetIdFeature, None).isDefined
  
  +        hasTopicSocialContext(candidate.features)
  
         }.map(_.candidate.id).toSet
  
   
  
       val (kept, removed) =
  
     private def hasFollowedBySocialContext(candidateFeatures: FeatureMap): Boolean =
  
       candidateFeatures.getOrElse(SGSValidFollowedByUserIdsFeature, Seq.empty).nonEmpty
  
   
  
  -  private def hasTopicSocialContext(candidateFeatures: FeatureMap): Boolean =
  
  +  private def hasTopicSocialContext(candidateFeatures: FeatureMap): Boolean = {
  
       candidateFeatures.getOrElse(TopicIdSocialContextFeature, None).isDefined &&
  
  -      candidateFeatures.getOrElse(TopicContextFunctionalityTypeFeature, None).isDefined
  
  +    candidateFeatures.getOrElse(TopicContextFunctionalityTypeFeature, None).isDefined
  
  +  }
  
   }
  
