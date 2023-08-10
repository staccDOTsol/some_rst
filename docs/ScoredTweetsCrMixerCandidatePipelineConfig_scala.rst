a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/ScoredTweetsCrMixerCandidatePipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/ScoredTweetsTweetMixerCandidatePipelineConfig.scala
==================================================

Change Range: -92,7 +94,7

Content:

::

similarity index 70%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/ScoredTweetsCrMixerCandidatePipelineConfig.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/ScoredTweetsTweetMixerCandidatePipelineConfig.scala
  
  index 1ec9353f8..df16e3f11 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/ScoredTweetsCrMixerCandidatePipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/ScoredTweetsTweetMixerCandidatePipelineConfig.scala
  
   package com.twitter.home_mixer.product.scored_tweets.candidate_pipeline
  
   
  
  -import com.twitter.cr_mixer.{thriftscala => t}
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.TweetypieStaticEntitiesFeatureHydrator
  
  -import com.twitter.home_mixer.functional_component.filter.PredicateFeatureFilter
  
  -import com.twitter.home_mixer.functional_component.gate.MinCachedTweetsGate
  
  +import com.twitter.tweet_mixer.{thriftscala => t}
  
   import com.twitter.home_mixer.model.HomeFeatures.AuthorIdFeature
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.TweetypieStaticEntitiesFeatureHydrator
  
  +import com.twitter.home_mixer.product.scored_tweets.gate.MinCachedTweetsGate
  
   import com.twitter.home_mixer.product.scored_tweets.model.ScoredTweetsQuery
  
   import com.twitter.home_mixer.product.scored_tweets.param.ScoredTweetsParam.CachedScoredTweets
  
  -import com.twitter.home_mixer.product.scored_tweets.param.ScoredTweetsParam.CrMixerSource
  
  -import com.twitter.home_mixer.product.scored_tweets.response_transformer.ScoredTweetsCrMixerResponseFeatureTransformer
  
  +import com.twitter.home_mixer.product.scored_tweets.param.ScoredTweetsParam.CandidatePipeline
  
  +import com.twitter.home_mixer.product.scored_tweets.response_transformer.ScoredTweetsTweetMixerResponseFeatureTransformer
  
   import com.twitter.home_mixer.util.CachedScoredTweetsHelper
  
  -import com.twitter.product_mixer.component_library.candidate_source.cr_mixer.CrMixerTweetRecommendationsCandidateSource
  
  +import com.twitter.product_mixer.component_library.candidate_source.tweet_mixer.TweetMixerCandidateSource
  
  +import com.twitter.product_mixer.component_library.filter.PredicateFeatureFilter
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.functional_component.candidate_source.BaseCandidateSource
  
   import com.twitter.product_mixer.core.functional_component.feature_hydrator.BaseCandidateFeatureHydrator
  
   import com.twitter.product_mixer.core.model.common.identifier.FilterIdentifier
  
   import com.twitter.product_mixer.core.pipeline.candidate.CandidatePipelineConfig
  
   import com.twitter.timelines.configapi.decider.DeciderParam
  
  +
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
   /**
  
  - * Candidate Pipeline Config that fetches tweets from CrMixer.
  
  + * Candidate Pipeline Config that fetches tweets from TweetMixer.
  
    */
  
   @Singleton
  
  -class ScoredTweetsCrMixerCandidatePipelineConfig @Inject() (
  
  -  crMixerTweetRecommendationsCandidateSource: CrMixerTweetRecommendationsCandidateSource,
  
  +class ScoredTweetsTweetMixerCandidatePipelineConfig @Inject() (
  
  +  tweetMixerCandidateSource: TweetMixerCandidateSource,
  
     tweetypieStaticEntitiesFeatureHydrator: TweetypieStaticEntitiesFeatureHydrator)
  
       extends CandidatePipelineConfig[
  
         ScoredTweetsQuery,
  
  -      t.CrMixerTweetRequest,
  
  -      t.TweetRecommendation,
  
  +      t.TweetMixerRequest,
  
  +      t.TweetResult,
  
         TweetCandidate
  
       ] {
  
   
  
     override val identifier: CandidatePipelineIdentifier =
  
  -    CandidatePipelineIdentifier("ScoredTweetsCrMixer")
  
  +    CandidatePipelineIdentifier("ScoredTweetsTweetMixer")
  
   
  
     val HasAuthorFilterId = "HasAuthor"
  
   
  
     override val enabledDeciderParam: Option[DeciderParam[Boolean]] =
  
  -    Some(CrMixerSource.EnableCandidatePipelineParam)
  
  +    Some(CandidatePipeline.EnableTweetMixerParam)
  
   
  
     override val gates: Seq[Gate[ScoredTweetsQuery]] = Seq(
  
  -    MinCachedTweetsGate(identifier, CachedScoredTweets.MinCachedTweetsParam)
  
  +    MinCachedTweetsGate(identifier, CachedScoredTweets.MinCachedTweetsParam),
  
     )
  
   
  
  -  override val candidateSource: BaseCandidateSource[t.CrMixerTweetRequest, t.TweetRecommendation] =
  
  -    crMixerTweetRecommendationsCandidateSource
  
  +  override val candidateSource: BaseCandidateSource[t.TweetMixerRequest, t.TweetResult] =
  
  +    tweetMixerCandidateSource
  
   
  
  -  private val MaxTweetsToFetch = 500
  
  +  private val MaxTweetsToFetch = 400
  
   
  
     override val queryTransformer: CandidatePipelineQueryTransformer[
  
       ScoredTweetsQuery,
  
  -    t.CrMixerTweetRequest
  
  +    t.TweetMixerRequest
  
     ] = { query =>
  
       val maxCount = (query.getQualityFactorCurrentValue(identifier) * MaxTweetsToFetch).toInt
  
   
  
       val excludedTweetIds = query.features.map(
  
         CachedScoredTweetsHelper.tweetImpressionsAndCachedScoredTweets(_, identifier))
  
   
  
  -    t.CrMixerTweetRequest(
  
  +    t.TweetMixerRequest(
  
         clientContext = ClientContextMarshaller(query.clientContext),
  
  -      product = t.Product.Home,
  
  -      productContext =
  
  -        Some(t.ProductContext.HomeContext(t.HomeContext(maxResults = Some(maxCount)))),
  
  -      excludedTweetIds = excludedTweetIds
  
  +      product = t.Product.HomeRecommendedTweets,
  
  +      productContext = Some(
  
  +        t.ProductContext.HomeRecommendedTweetsProductContext(
  
  +          t.HomeRecommendedTweetsProductContext(excludedTweetIds = excludedTweetIds.map(_.toSet)))),
  
  +      maxResults = Some(maxCount)
  
       )
  
     }
  
   
  
     ] = Seq(tweetypieStaticEntitiesFeatureHydrator)
  
   
  
     override val featuresFromCandidateSourceTransformers: Seq[
  
  -    CandidateFeatureTransformer[t.TweetRecommendation]
  
  -  ] = Seq(ScoredTweetsCrMixerResponseFeatureTransformer)
  
  +    CandidateFeatureTransformer[t.TweetResult]
  
  +  ] = Seq(ScoredTweetsTweetMixerResponseFeatureTransformer)
  
   
  
     override val filters: Seq[Filter[ScoredTweetsQuery, TweetCandidate]] = Seq(
  
       PredicateFeatureFilter.fromPredicate(
  
     )
  
   
  
     override val resultTransformer: CandidatePipelineResultsTransformer[
  
  -    t.TweetRecommendation,
  
  +    t.TweetResult,
  
       TweetCandidate
  
     ] = { sourceResult => TweetCandidate(id = sourceResult.tweetId) }
  
   }
  
