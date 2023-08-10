a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/response_transformer/TweetPreviewResponseFeatureTransformer.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/response_transformer/TweetPreviewResponseFeatureTransformer.scala
==================================================

Change Range: -0,0 +1,32

Content:

::

new file mode 100644
  
  index 000000000..8b783db38
  
  --- /dev/null
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/response_transformer/TweetPreviewResponseFeatureTransformer.scala
  
  +package com.twitter.home_mixer.product.for_you.response_transformer
  
  +
  
  +import com.twitter.home_mixer.model.HomeFeatures.AuthorIdFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.IsTweetPreviewFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.SuggestTypeFeature
  
  +import com.twitter.product_mixer.core.feature.Feature
  
  +import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
  +import com.twitter.product_mixer.core.feature.featuremap.FeatureMapBuilder
  
  +import com.twitter.product_mixer.core.functional_component.transformer.CandidateFeatureTransformer
  
  +import com.twitter.product_mixer.core.model.common.identifier.TransformerIdentifier
  
  +import com.twitter.timelineservice.suggests.{thriftscala => st}
  
  +import com.twitter.search.earlybird.{thriftscala => eb}
  
  +
  
  +object TweetPreviewResponseFeatureTransformer
  
  +    extends CandidateFeatureTransformer[eb.ThriftSearchResult] {
  
  +
  
  +  override val identifier: TransformerIdentifier =
  
  +    TransformerIdentifier("TweetPreviewResponse")
  
  +
  
  +  override val features: Set[Feature[_, _]] =
  
  +    Set(AuthorIdFeature, IsTweetPreviewFeature, SuggestTypeFeature)
  
  +
  
  +  def transform(
  
  +    input: eb.ThriftSearchResult
  
  +  ): FeatureMap = {
  
  +    FeatureMapBuilder()
  
  +      .add(IsTweetPreviewFeature, true)
  
  +      .add(SuggestTypeFeature, Some(st.SuggestType.TweetPreview))
  
  +      .add(AuthorIdFeature, input.metadata.map(_.fromUserId))
  
  +      .build()
  
  +  }
  
  +}
  
