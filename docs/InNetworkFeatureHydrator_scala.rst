a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/InNetworkFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/InNetworkFeatureHydrator.scala
==================================================

Change Range: -0,0 +1,41

Content:

::

new file mode 100644
  
  index 000000000..55f002b1f
  
  --- /dev/null
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/InNetworkFeatureHydrator.scala
  
  +package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +
  
  +import com.twitter.home_mixer.model.HomeFeatures.AuthorIdFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.InNetworkFeature
  
  +import com.twitter.product_mixer.component_library.feature_hydrator.query.social_graph.SGSFollowedUsersFeature
  
  +import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
  +import com.twitter.product_mixer.core.feature.Feature
  
  +import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
  +import com.twitter.product_mixer.core.feature.featuremap.FeatureMapBuilder
  
  +import com.twitter.product_mixer.core.functional_component.feature_hydrator.BulkCandidateFeatureHydrator
  
  +import com.twitter.product_mixer.core.model.common.CandidateWithFeatures
  
  +import com.twitter.product_mixer.core.model.common.identifier.FeatureHydratorIdentifier
  
  +import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  +import com.twitter.stitch.Stitch
  
  +
  
  +object InNetworkFeatureHydrator
  
  +    extends BulkCandidateFeatureHydrator[PipelineQuery, TweetCandidate] {
  
  +
  
  +  override val identifier: FeatureHydratorIdentifier = FeatureHydratorIdentifier("InNetwork")
  
  +
  
  +  override val features: Set[Feature[_, _]] = Set(InNetworkFeature)
  
  +
  
  +  override def apply(
  
  +    query: PipelineQuery,
  
  +    candidates: Seq[CandidateWithFeatures[TweetCandidate]]
  
  +  ): Stitch[Seq[FeatureMap]] = {
  
  +    val viewerId = query.getRequiredUserId
  
  +    val followedUserIds = query.features.get.get(SGSFollowedUsersFeature).toSet
  
  +
  
  +    val featureMaps = candidates.map { candidate =>
  
  +      // We use authorId and not sourceAuthorId here so that retweets are defined as in network
  
  +      val isInNetworkOpt = candidate.features.getOrElse(AuthorIdFeature, None).map { authorId =>
  
  +        // Users cannot follow themselves but this is in network by definition
  
  +        val isSelfTweet = authorId == viewerId
  
  +        isSelfTweet || followedUserIds.contains(authorId)
  
  +      }
  
  +      FeatureMapBuilder().add(InNetworkFeature, isInNetworkOpt.getOrElse(true)).build()
  
  +    }
  
  +    Stitch.value(featureMaps)
  
  +  }
  
  +}
  
