a/home-mixer/server/src/main/scala/com/twitter/home_mixer/model/request/HomeMixerProduct.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/model/request/HomeMixerProduct.scala
==================================================

Change Range: -33,3 +33,8

Content:

::

index 107f4c243..7c27c50d5 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/model/request/HomeMixerProduct.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/model/request/HomeMixerProduct.scala
  
     override val identifier: ProductIdentifier = ProductIdentifier("ListRecommendedUsers")
  
     override val stringCenterProject: Option[String] = Some("timelinemixer")
  
   }
  
  +
  
  +case object SubscribedProduct extends Product {
  
  +  override val identifier: ProductIdentifier = ProductIdentifier("Subscribed")
  
  +  override val stringCenterProject: Option[String] = Some("timelinemixer")
  
  +}
  
