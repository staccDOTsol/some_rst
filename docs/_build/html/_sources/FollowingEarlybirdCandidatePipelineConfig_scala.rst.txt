a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingEarlybirdCandidatePipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingEarlybirdCandidatePipelineConfig.scala
==================================================

Change Range: -2,9 +2,9

Content:

::

index ea26ca19e..addb298c2 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingEarlybirdCandidatePipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingEarlybirdCandidatePipelineConfig.scala
  
   
  
   import com.twitter.home_mixer.candidate_pipeline.FollowingEarlybirdResponseFeatureTransformer
  
   import com.twitter.home_mixer.functional_component.candidate_source.EarlybirdCandidateSource
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.SGSFollowedUsersFeature
  
  -import com.twitter.home_mixer.functional_component.gate.NonEmptySeqFeatureGate
  
   import com.twitter.home_mixer.product.following.model.FollowingQuery
  
  +import com.twitter.product_mixer.component_library.feature_hydrator.query.social_graph.SGSFollowedUsersFeature
  
  +import com.twitter.product_mixer.component_library.gate.NonEmptySeqFeatureGate
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.functional_component.candidate_source.BaseCandidateSource
  
   import com.twitter.product_mixer.core.functional_component.gate.Gate
  
