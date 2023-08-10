a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/UtegFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/UtegFeatureHydrator.scala
==================================================

Change Range: -78,10 +84,18

Content:

::

similarity index 77%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/UtegFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/UtegFeatureHydrator.scala
  
  index 389a50d03..96b9657e7 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/UtegFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/UtegFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator
  
   
  
  +import com.twitter.home_mixer.model.HomeFeatures.FavoritedByCountFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.FavoritedByUserIdsFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.FromInNetworkSourceFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.InReplyToTweetIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.RealGraphInNetworkScoresFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.RepliedByCountFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.RepliedByEngagerIdsFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.RetweetedByCountFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.RetweetedByEngagerIdsFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.SourceTweetIdFeature
  
   import com.twitter.home_mixer.param.HomeMixerInjectionNames.UtegSocialProofRepository
  
   import com.twitter.product_mixer.core.model.common.Conditionally
  
   import com.twitter.product_mixer.core.model.common.identifier.FeatureHydratorIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  +import com.twitter.product_mixer.core.util.OffloadFuturePools
  
   import com.twitter.recos.recos_common.{thriftscala => rc}
  
   import com.twitter.recos.user_tweet_entity_graph.{thriftscala => uteg}
  
   import com.twitter.servo.keyvalue.KeyValueResult
  
   import com.twitter.servo.repository.KeyValueRepository
  
   import com.twitter.stitch.Stitch
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Named
  
   import javax.inject.Singleton
  
     override val features: Set[Feature[_, _]] = Set(
  
       FavoritedByUserIdsFeature,
  
       RetweetedByEngagerIdsFeature,
  
  -    RepliedByEngagerIdsFeature
  
  +    RepliedByEngagerIdsFeature,
  
  +    FavoritedByCountFeature,
  
  +    RetweetedByCountFeature,
  
  +    RepliedByCountFeature
  
     )
  
   
  
     override def onlyIf(query: PipelineQuery): Boolean = query.features
  
     override def apply(
  
       query: PipelineQuery,
  
       candidates: Seq[CandidateWithFeatures[TweetCandidate]]
  
  -  ): Stitch[Seq[FeatureMap]] = {
  
  +  ): Stitch[Seq[FeatureMap]] = OffloadFuturePools.offloadFuture {
  
       val seedUserWeights = query.features.map(_.get(RealGraphInNetworkScoresFeature)).get
  
   
  
       val sourceTweetIds = candidates.flatMap(_.features.getOrElse(SourceTweetIdFeature, None))
  
   
  
       val utegQuery = (tweetIdsToSend, (query.getRequiredUserId, seedUserWeights))
  
   
  
  -    Stitch
  
  -      .callFuture(client(utegQuery))
  
  -      .map(handleResponse(candidates, _))
  
  +    client(utegQuery).map(handleResponse(candidates, _))
  
     }
  
   
  
     private def handleResponse(
  
       results: KeyValueResult[Long, uteg.TweetRecommendation],
  
     ): Seq[FeatureMap] = {
  
       candidates.map { candidate =>
  
  +      val inNetwork = candidate.features.getOrElse(FromInNetworkSourceFeature, false)
  
         val candidateProof = results(candidate.candidate.id).toOption.flatten
  
         val sourceProof = candidate.features
  
           .getOrElse(SourceTweetIdFeature, None).flatMap(results(_).toOption.flatten)
  
         val retweetedBy = proofs.flatMap(_.get(rc.SocialProofType.Retweet)).flatten
  
         val repliedBy = proofs.flatMap(_.get(rc.SocialProofType.Reply)).flatten
  
   
  
  +      val (favoritedByCount, retweetedByCount, repliedByCount) =
  
  +        if (!inNetwork) {
  
  +          (favoritedBy.size.toDouble, retweetedBy.size.toDouble, repliedBy.size.toDouble)
  
  +        } else { (0.0, 0.0, 0.0) }
  
  +
  
         FeatureMapBuilder()
  
           .add(FavoritedByUserIdsFeature, favoritedBy)
  
           .add(RetweetedByEngagerIdsFeature, retweetedBy)
  
           .add(RepliedByEngagerIdsFeature, repliedBy)
  
  +        .add(FavoritedByCountFeature, favoritedByCount)
  
  +        .add(RetweetedByCountFeature, retweetedByCount)
  
  +        .add(RepliedByCountFeature, repliedByCount)
  
           .build()
  
       }
  
     }
  
