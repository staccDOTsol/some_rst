a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/response_transformer/earlybird/ScoredTweetsEarlybirdFrsResponseFeatureTransformer.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/response_transformer/earlybird/ScoredTweetsEarlybirdFrsResponseFeatureTransformer.scala
==================================================

Change Range: -0,0 +1,33

Content:

::

new file mode 100644
  
  index 000000000..bb9ea8bee
  
  --- /dev/null
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/response_transformer/earlybird/ScoredTweetsEarlybirdFrsResponseFeatureTransformer.scala
  
  +package com.twitter.home_mixer.product.scored_tweets.response_transformer.earlybird
  
  +
  
  +import com.twitter.home_mixer.model.HomeFeatures.CandidateSourceIdFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.SuggestTypeFeature
  
  +import com.twitter.product_mixer.core.feature.Feature
  
  +import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
  +import com.twitter.product_mixer.core.feature.featuremap.FeatureMapBuilder
  
  +import com.twitter.product_mixer.core.functional_component.transformer.CandidateFeatureTransformer
  
  +import com.twitter.product_mixer.core.model.common.identifier.TransformerIdentifier
  
  +import com.twitter.search.earlybird.{thriftscala => eb}
  
  +import com.twitter.timelineservice.suggests.logging.candidate_tweet_source_id.{thriftscala => cts}
  
  +import com.twitter.timelineservice.suggests.{thriftscala => st}
  
  +
  
  +object ScoredTweetsEarlybirdFrsResponseFeatureTransformer
  
  +    extends CandidateFeatureTransformer[eb.ThriftSearchResult] {
  
  +
  
  +  override val identifier: TransformerIdentifier =
  
  +    TransformerIdentifier("ScoredTweetsEarlybirdFrsResponse")
  
  +
  
  +  override val features: Set[Feature[_, _]] = EarlybirdResponseTransformer.features
  
  +
  
  +  override def transform(candidate: eb.ThriftSearchResult): FeatureMap = {
  
  +
  
  +    val baseFeatures = EarlybirdResponseTransformer.transform(candidate)
  
  +
  
  +    val features = FeatureMapBuilder()
  
  +      .add(CandidateSourceIdFeature, Some(cts.CandidateTweetSourceId.FrsTweet))
  
  +      .add(SuggestTypeFeature, Some(st.SuggestType.FrsTweet))
  
  +      .build()
  
  +
  
  +    baseFeatures ++ features
  
  +  }
  
  +}
  
