a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/functional_component/gate/PushToHomeRequestGate.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/functional_component/gate/PushToHomeRequestGate.scala
==================================================

Change Range: -0,0 +1,16

Content:

::

new file mode 100644
  
  index 000000000..4c9d81021
  
  --- /dev/null
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/functional_component/gate/PushToHomeRequestGate.scala
  
  +package com.twitter.home_mixer.product.for_you.functional_component.gate
  
  +
  
  +import com.twitter.home_mixer.product.for_you.model.ForYouQuery
  
  +import com.twitter.product_mixer.core.functional_component.gate.Gate
  
  +import com.twitter.product_mixer.core.model.common.identifier.GateIdentifier
  
  +import com.twitter.stitch.Stitch
  
  +
  
  +/**
  
  + * Continues when the request is a Push-To-Home notification request
  
  + */
  
  +object PushToHomeRequestGate extends Gate[ForYouQuery] {
  
  +  override val identifier: GateIdentifier = GateIdentifier("PushToHomeRequest")
  
  +
  
  +  override def shouldContinue(query: ForYouQuery): Stitch[Boolean] =
  
  +    Stitch.value(query.pushToHomeTweetId.isDefined)
  
  +}
  
