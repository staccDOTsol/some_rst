a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/response_transformer/ScoredTweetsInNetworkResponseFeatureTransformer.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/response_transformer/ScoredTweetsInNetworkResponseFeatureTransformer.scala
==================================================

Change Range: -26,7 +26,7

Content:

::

index ada45d3e1..d7d23bc98 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/response_transformer/ScoredTweetsInNetworkResponseFeatureTransformer.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/response_transformer/ScoredTweetsInNetworkResponseFeatureTransformer.scala
  
       val features = FeatureMapBuilder()
  
         .add(CandidateSourceIdFeature, Some(cts.CandidateTweetSourceId.RecycledTweet))
  
         .add(FromInNetworkSourceFeature, true)
  
  -      .add(SuggestTypeFeature, Some(st.SuggestType.RecycledTweetInline))
  
  +      .add(SuggestTypeFeature, Some(st.SuggestType.RankedTimelineTweet))
  
         .build()
  
   
  
       baseFeatures ++ features
  
