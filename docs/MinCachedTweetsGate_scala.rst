a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/gate/MinCachedTweetsGate.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/gate/MinCachedTweetsGate.scala
==================================================

Change Range: -1,6 +1,6

Content:

::

similarity index 89%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/gate/MinCachedTweetsGate.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/gate/MinCachedTweetsGate.scala
  
  index a160a5b5d..bdeb33a92 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/gate/MinCachedTweetsGate.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/gate/MinCachedTweetsGate.scala
  
  -package com.twitter.home_mixer.functional_component.gate
  
  +package com.twitter.home_mixer.product.scored_tweets.gate
  
   
  
  -import com.twitter.home_mixer.functional_component.gate.MinCachedTweetsGate.identifierSuffix
  
  +import com.twitter.home_mixer.product.scored_tweets.gate.MinCachedTweetsGate.identifierSuffix
  
   import com.twitter.home_mixer.util.CachedScoredTweetsHelper
  
   import com.twitter.product_mixer.core.functional_component.gate.Gate
  
   import com.twitter.product_mixer.core.model.common.identifier.CandidatePipelineIdentifier
  
