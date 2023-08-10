a/home-mixer/server/src/main/scala/com/twitter/home_mixer/model/ContentFeatures.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/model/ContentFeatures.scala
==================================================

Change Range: -48,6 +49,53

Content:

::

index 44e8cf25e..f141d67d1 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/model/ContentFeatures.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/model/ContentFeatures.scala
  
   package com.twitter.home_mixer.model
  
   
  
   import com.twitter.escherbird.{thriftscala => esb}
  
  +import com.twitter.search.common.features.{thriftscala => sc}
  
   import com.twitter.tweetypie.{thriftscala => tp}
  
   
  
   object ContentFeatures {
  
       None,
  
       None
  
     )
  
  +
  
  +  def fromThrift(ebFeatures: sc.ThriftTweetFeatures): ContentFeatures =
  
  +    ContentFeatures(
  
  +      length = ebFeatures.tweetLength.getOrElse(0).toShort,
  
  +      hasQuestion = ebFeatures.hasQuestion.getOrElse(false),
  
  +      numCaps = ebFeatures.numCaps.getOrElse(0).toShort,
  
  +      numWhiteSpaces = ebFeatures.numWhitespaces.getOrElse(0).toShort,
  
  +      numNewlines = ebFeatures.numNewlines,
  
  +      videoDurationMs = ebFeatures.videoDurationMs,
  
  +      bitRate = ebFeatures.bitRate,
  
  +      aspectRatioNum = ebFeatures.aspectRatioNum,
  
  +      aspectRatioDen = ebFeatures.aspectRatioDen,
  
  +      widths = ebFeatures.widths.map(_.map(_.toShort)),
  
  +      heights = ebFeatures.heights.map(_.map(_.toShort)),
  
  +      resizeMethods = ebFeatures.resizeMethods.map(_.map(_.toShort)),
  
  +      numMediaTags = ebFeatures.numMediaTags.map(_.toShort),
  
  +      mediaTagScreenNames = ebFeatures.mediaTagScreenNames,
  
  +      emojiTokens = ebFeatures.emojiTokens.map(_.toSet),
  
  +      emoticonTokens = ebFeatures.emoticonTokens.map(_.toSet),
  
  +      faceAreas = ebFeatures.faceAreas,
  
  +      dominantColorRed = ebFeatures.dominantColorRed,
  
  +      dominantColorBlue = ebFeatures.dominantColorBlue,
  
  +      dominantColorGreen = ebFeatures.dominantColorGreen,
  
  +      numColors = ebFeatures.numColors.map(_.toShort),
  
  +      stickerIds = ebFeatures.stickerIds,
  
  +      mediaOriginProviders = ebFeatures.mediaOriginProviders,
  
  +      isManaged = ebFeatures.isManaged,
  
  +      is360 = ebFeatures.is360,
  
  +      viewCount = ebFeatures.viewCount,
  
  +      isMonetizable = ebFeatures.isMonetizable,
  
  +      isEmbeddable = ebFeatures.isEmbeddable,
  
  +      hasSelectedPreviewImage = ebFeatures.hasSelectedPreviewImage,
  
  +      hasTitle = ebFeatures.hasTitle,
  
  +      hasDescription = ebFeatures.hasDescription,
  
  +      hasVisitSiteCallToAction = ebFeatures.hasVisitSiteCallToAction,
  
  +      hasAppInstallCallToAction = ebFeatures.hasAppInstallCallToAction,
  
  +      hasWatchNowCallToAction = ebFeatures.hasWatchNowCallToAction,
  
  +      dominantColorPercentage = ebFeatures.dominantColorPercentage,
  
  +      posUnigrams = ebFeatures.posUnigrams.map(_.toSet),
  
  +      posBigrams = ebFeatures.posBigrams.map(_.toSet),
  
  +      semanticCoreAnnotations = ebFeatures.semanticCoreAnnotations,
  
  +      tokens = ebFeatures.textTokens.map(_.toSeq),
  
  +      conversationControl = ebFeatures.conversationControl,
  
  +      // media and selfThreadMetadata not carried by ThriftTweetFeatures
  
  +      media = None,
  
  +      selfThreadMetadata = None
  
  +    )
  
   }
  
   
  
   case class ContentFeatures(
  
