a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/model/HomeMixerExternalStrings.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/model/HomeMixerExternalStrings.scala
==================================================

Change Range: -63,4 +63,25

Content:

::

index fb07bc12c..9c6faafa7 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/model/HomeMixerExternalStrings.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/model/HomeMixerExternalStrings.scala
  
       externalStringRegistryProvider.get().createProdString("SocialContext.extendedReply")
  
     val socialContextReceivedReply =
  
       externalStringRegistryProvider.get().createProdString("SocialContext.receivedReply")
  
  +
  
  +  val socialContextPopularVideoString =
  
  +    externalStringRegistryProvider.get().createProdString("SocialContext.popularVideo")
  
  +
  
  +  val socialContextPopularInYourAreaString =
  
  +    externalStringRegistryProvider.get().createProdString("PopgeoTweet.socialProof")
  
  +
  
  +  val listToFollowModuleHeaderString =
  
  +    externalStringRegistryProvider.get().createProdString("ListToFollowModule.header")
  
  +  val listToFollowModuleFooterString =
  
  +    externalStringRegistryProvider.get().createProdString("ListToFollowModule.footer")
  
  +  val pinnedListsModuleHeaderString =
  
  +    externalStringRegistryProvider.get().createProdString("PinnedListModule.header")
  
  +  val pinnedListsModuleEmptyStateMessageString =
  
  +    externalStringRegistryProvider.get().createProdString("PinnedListModule.emptyStateMessage")
  
  +
  
  +  val ownedSubscribedListsModuleHeaderString =
  
  +    externalStringRegistryProvider.get().createProdString("OwnedSubscribedListModule.header")
  
  +  val ownedSubscribedListsModuleEmptyStateMessageString =
  
  +    externalStringRegistryProvider
  
  +      .get().createProdString("OwnedSubscribedListModule.emptyStateMessage")
  
   }
  
