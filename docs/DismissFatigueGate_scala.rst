a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/gate/DismissFatigueGate.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/gate/DismissFatigueGate.scala
==================================================

Change Range: -1,14 +1,14

Content:

::

index c97a3931e..b35a6cda5 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/gate/DismissFatigueGate.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/gate/DismissFatigueGate.scala
  
   package com.twitter.home_mixer.functional_component.gate
  
   
  
  +import com.twitter.conversions.DurationOps._
  
  +import com.twitter.product_mixer.core.feature.Feature
  
   import com.twitter.product_mixer.core.functional_component.gate.Gate
  
   import com.twitter.product_mixer.core.model.common.identifier.GateIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  -import com.twitter.timelinemixer.clients.manhattan.DismissInfo
  
   import com.twitter.stitch.Stitch
  
  -import com.twitter.util.Duration
  
  -import com.twitter.conversions.DurationOps._
  
  -import com.twitter.product_mixer.core.feature.Feature
  
  +import com.twitter.timelinemixer.clients.manhattan.DismissInfo
  
   import com.twitter.timelineservice.suggests.thriftscala.SuggestType
  
  +import com.twitter.util.Duration
  
   
  
   object DismissFatigueGate {
  
     // how long a dismiss action from user needs to be respected
  
