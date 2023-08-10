a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/AncestorFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/AncestorFeatureHydrator.scala
==================================================

Change Range: -40,7 +44,8

Content:

::

similarity index 75%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/AncestorFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/AncestorFeatureHydrator.scala
  
  index 8d1a0862d..9c5e8f3ab 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/AncestorFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/AncestorFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator
  
   
  
   import com.twitter.home_mixer.model.HomeFeatures.AncestorsFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.InReplyToTweetIdFeature
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.feature.Feature
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
   import com.twitter.product_mixer.core.functional_component.feature_hydrator.CandidateFeatureHydrator
  
   import com.twitter.product_mixer.core.model.common.identifier.FeatureHydratorIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  +import com.twitter.product_mixer.core.util.OffloadFuturePools
  
   import com.twitter.stitch.Stitch
  
   import com.twitter.tweetconvosvc.tweet_ancestor.{thriftscala => ta}
  
   import com.twitter.tweetconvosvc.{thriftscala => tcs}
  
  +import com.twitter.util.Future
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
   
  
     override val features: Set[Feature[_, _]] = Set(AncestorsFeature)
  
   
  
  +  private val DefaultFeatureMap = FeatureMapBuilder().add(AncestorsFeature, Seq.empty).build()
  
  +
  
     override def apply(
  
       query: PipelineQuery,
  
       candidate: TweetCandidate,
  
       existingFeatures: FeatureMap
  
  -  ): Stitch[FeatureMap] = {
  
  -    val ancestorsRequest = tcs.GetAncestorsRequest(Seq(candidate.id))
  
  -
  
  -    Stitch.callFuture(conversationServiceClient.getAncestors(ancestorsRequest)).map {
  
  -      getAncestorsResponse =>
  
  +  ): Stitch[FeatureMap] = OffloadFuturePools.offloadFuture {
  
  +    if (existingFeatures.getOrElse(InReplyToTweetIdFeature, None).isDefined) {
  
  +      val ancestorsRequest = tcs.GetAncestorsRequest(Seq(candidate.id))
  
  +      conversationServiceClient.getAncestors(ancestorsRequest).map { getAncestorsResponse =>
  
           val ancestors = getAncestorsResponse.ancestors.headOption
  
             .collect {
  
               case tcs.TweetAncestorsResult.TweetAncestors(ancestorsResult)
  
             }.getOrElse(Seq.empty)
  
   
  
           FeatureMapBuilder().add(AncestorsFeature, ancestors).build()
  
  -    }
  
  +      }
  
  +    } else Future.value(DefaultFeatureMap)
  
     }
  
   
  
     private def getTruncatedRootTweet(
  
