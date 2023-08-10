a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/EarlybirdFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/EarlybirdFeatureHydrator.scala
==================================================

Change Range: -122,6 +143,7

Content:

::

similarity index 68%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/EarlybirdFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/EarlybirdFeatureHydrator.scala
  
  index 639c3ac37..cc3c2aeeb 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/EarlybirdFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/EarlybirdFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator
  
   
  
   import com.twitter.finagle.stats.StatsReceiver
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.adapters.earlybird.EarlybirdAdapter
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.adapters.earlybird.EarlybirdAdapter
  
   import com.twitter.home_mixer.model.HomeFeatures.DeviceLanguageFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.EarlybirdFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.EarlybirdSearchResultFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.IsRetweetFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.TweetUrlsFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.UserScreenNameFeature
  
   import com.twitter.home_mixer.util.ObservedKeyValueResultHandler
  
   import com.twitter.home_mixer.util.earlybird.EarlybirdResponseUtil
  
   import com.twitter.ml.api.DataRecord
  
  +import com.twitter.product_mixer.component_library.feature_hydrator.query.social_graph.SGSFollowedUsersFeature
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.feature.Feature
  
   import com.twitter.product_mixer.core.feature.FeatureWithDefaultOnFailure
  
   import com.twitter.product_mixer.core.model.common.CandidateWithFeatures
  
   import com.twitter.product_mixer.core.model.common.identifier.FeatureHydratorIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  +import com.twitter.product_mixer.core.util.OffloadFuturePools
  
   import com.twitter.search.earlybird.{thriftscala => eb}
  
   import com.twitter.servo.keyvalue.KeyValueResult
  
   import com.twitter.servo.repository.KeyValueRepository
  
   
  
     override val identifier: FeatureHydratorIdentifier = FeatureHydratorIdentifier("Earlybird")
  
   
  
  -  override val features: Set[Feature[_, _]] =
  
  -    Set(EarlybirdDataRecordFeature, EarlybirdFeature, TweetUrlsFeature)
  
  +  override val features: Set[Feature[_, _]] = Set(
  
  +    EarlybirdDataRecordFeature,
  
  +    EarlybirdFeature,
  
  +    EarlybirdSearchResultFeature,
  
  +    TweetUrlsFeature
  
  +  )
  
   
  
     override val statScope: String = identifier.toString
  
   
  
     private val originalKeyFoundCounter = scopedStatsReceiver.counter("originalKey/found")
  
     private val originalKeyLossCounter = scopedStatsReceiver.counter("originalKey/loss")
  
   
  
  +  private val ebSearchResultNotExistPredicate: CandidateWithFeatures[TweetCandidate] => Boolean =
  
  +    candidate => candidate.features.getOrElse(EarlybirdSearchResultFeature, None).isEmpty
  
     private val ebFeaturesNotExistPredicate: CandidateWithFeatures[TweetCandidate] => Boolean =
  
       candidate => candidate.features.getOrElse(EarlybirdFeature, None).isEmpty
  
   
  
     override def apply(
  
       query: PipelineQuery,
  
       candidates: Seq[CandidateWithFeatures[TweetCandidate]]
  
  -  ): Stitch[Seq[FeatureMap]] = {
  
  +  ): Stitch[Seq[FeatureMap]] = OffloadFuturePools.offloadFuture {
  
       val candidatesToHydrate = candidates.filter { candidate =>
  
  -      val isEmpty = ebFeaturesNotExistPredicate(candidate)
  
  +      val isEmpty =
  
  +        ebFeaturesNotExistPredicate(candidate) && ebSearchResultNotExistPredicate(candidate)
  
         if (isEmpty) originalKeyLossCounter.incr() else originalKeyFoundCounter.incr()
  
         isEmpty
  
       }
  
  -    Stitch
  
  -      .callFuture(client((candidatesToHydrate.map(_.candidate.id), query.getRequiredUserId)))
  
  -      .map(handleResponse(query, candidates, _))
  
  +
  
  +    client((candidatesToHydrate.map(_.candidate.id), query.getRequiredUserId))
  
  +      .map(handleResponse(query, candidates, _, candidatesToHydrate))
  
     }
  
   
  
     private def handleResponse(
  
       query: PipelineQuery,
  
       candidates: Seq[CandidateWithFeatures[TweetCandidate]],
  
  -    results: KeyValueResult[Long, eb.ThriftSearchResult]
  
  +    results: KeyValueResult[Long, eb.ThriftSearchResult],
  
  +    candidatesToHydrate: Seq[CandidateWithFeatures[TweetCandidate]]
  
     ): Seq[FeatureMap] = {
  
       val queryFeatureMap = query.features.getOrElse(FeatureMap.empty)
  
       val userLanguages = queryFeatureMap.getOrElse(UserLanguagesFeature, Seq.empty)
  
       val uiLanguageCode = queryFeatureMap.getOrElse(DeviceLanguageFeature, None)
  
       val screenName = queryFeatureMap.getOrElse(UserScreenNameFeature, None)
  
  +    val followedUserIds = queryFeatureMap.getOrElse(SGSFollowedUsersFeature, Seq.empty).toSet
  
   
  
  -    val searchResults = candidates
  
  -      .filter(ebFeaturesNotExistPredicate).map { candidate =>
  
  +    val searchResults = candidatesToHydrate
  
  +      .map { candidate =>
  
           observedGet(Some(candidate.candidate.id), results)
  
         }.collect {
  
           case Return(Some(value)) => value
  
         }
  
   
  
  -    val tweetIdToEbFeatures = EarlybirdResponseUtil.getOONTweetThriftFeaturesByTweetId(
  
  +    val allSearchResults = searchResults ++
  
  +      candidates.filter(!ebSearchResultNotExistPredicate(_)).flatMap { candidate =>
  
  +        candidate.features
  
  +          .getOrElse(EarlybirdSearchResultFeature, None)
  
  +      }
  
  +    val idToSearchResults = allSearchResults.map(sr => sr.id -> sr).toMap
  
  +    val tweetIdToEbFeatures = EarlybirdResponseUtil.getTweetThriftFeaturesByTweetId(
  
         searcherUserId = query.getRequiredUserId,
  
         screenName = screenName,
  
         userLanguages = userLanguages,
  
         uiLanguageCode = uiLanguageCode,
  
  -      searchResults = searchResults
  
  +      followedUserIds = followedUserIds,
  
  +      mutuallyFollowingUserIds = Set.empty,
  
  +      searchResults = allSearchResults,
  
  +      sourceTweetSearchResults = Seq.empty,
  
       )
  
   
  
       candidates.map { candidate =>
  
  -      val hydratedEbFeatures = tweetIdToEbFeatures.get(candidate.candidate.id)
  
  +      val transformedEbFeatures = tweetIdToEbFeatures.get(candidate.candidate.id)
  
         val earlybirdFeatures =
  
  -        if (hydratedEbFeatures.nonEmpty) hydratedEbFeatures
  
  +        if (transformedEbFeatures.nonEmpty) transformedEbFeatures
  
           else candidate.features.getOrElse(EarlybirdFeature, None)
  
   
  
         val candidateIsRetweet = candidate.features.getOrElse(IsRetweetFeature, false)
  
         FeatureMapBuilder()
  
           .add(EarlybirdFeature, earlybirdFeatures)
  
           .add(EarlybirdDataRecordFeature, earlybirdDataRecord)
  
  +        .add(EarlybirdSearchResultFeature, idToSearchResults.get(candidate.candidate.id))
  
           .add(TweetUrlsFeature, earlybirdFeatures.flatMap(_.urlsList).getOrElse(Seq.empty))
  
           .build()
  
       }
  
