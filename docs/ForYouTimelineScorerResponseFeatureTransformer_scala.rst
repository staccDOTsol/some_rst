a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouTimelineScorerResponseFeatureTransformer.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouTimelineScorerResponseFeatureTransformer.scala
==================================================

Change Range: -151,6 +160,7

Content:

::

index dfae6a10f..236ca9a26 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouTimelineScorerResponseFeatureTransformer.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/ForYouTimelineScorerResponseFeatureTransformer.scala
  
       AncestorsFeature,
  
       AudioSpaceMetaDataFeature,
  
       AuthorIdFeature,
  
  -    AuthorIsEligibleForConnectBoostFeature,
  
  +    AuthorIsBlueVerifiedFeature,
  
  +    AuthorIsCreatorFeature,
  
  +    AuthorIsGoldVerifiedFeature,
  
  +    AuthorIsGrayVerifiedFeature,
  
  +    AuthorIsLegacyVerifiedFeature,
  
       AuthoredByContextualUserFeature,
  
       CandidateSourceIdFeature,
  
       ConversationFeature,
  
       FromInNetworkSourceFeature,
  
       FullScoringSucceededFeature,
  
       HasDisplayedTextFeature,
  
  +    InNetworkFeature,
  
       InReplyToTweetIdFeature,
  
       IsAncestorCandidateFeature,
  
       IsExtendedReplyFeature,
  
         val tcoLengthsPlusSpaces = 23 * numMedia + (if (numMedia > 0) numMedia - 1 else 0)
  
         length > tcoLengthsPlusSpaces
  
       }))
  
  -    val suggestType = Some(
  
  -      candidate.overrideSuggestType.getOrElse(tls.SuggestType.RankedTimelineTweet))
  
  +    val suggestType = candidate.overrideSuggestType.orElse(Some(tls.SuggestType.Undefined))
  
   
  
       val topicSocialProofMetadataOpt = candidate.entityData.flatMap(_.topicSocialProofMetadata)
  
       val topicIdSocialContextOpt = topicSocialProofMetadataOpt.map(_.topicId)
  
           AudioSpaceMetaDataFeature,
  
           candidate.audioSpaceMetaDatalist.map(_.head).map(AudioSpaceMetaData.fromThrift))
  
         .add(AuthorIdFeature, Some(candidate.authorId))
  
  +      .add(AuthorIsBlueVerifiedFeature, candidate.authorIsBlueVerified.getOrElse(false))
  
         .add(
  
  -        AuthorIsEligibleForConnectBoostFeature,
  
  -        candidate.authorIsEligibleForConnectBoost.getOrElse(false))
  
  +        AuthorIsCreatorFeature,
  
  +        candidate.authorIsCreator.getOrElse(false)
  
  +      )
  
  +      .add(AuthorIsGoldVerifiedFeature, candidate.authorIsGoldVerified.getOrElse(false))
  
  +      .add(AuthorIsGrayVerifiedFeature, candidate.authorIsGrayVerified.getOrElse(false))
  
  +      .add(AuthorIsLegacyVerifiedFeature, candidate.authorIsLegacyVerified.getOrElse(false))
  
         .add(
  
           AuthoredByContextualUserFeature,
  
           candidate.viewerId.contains(candidate.authorId) ||
  
         .add(TopicContextFunctionalityTypeFeature, topicContextFunctionalityTypeOpt)
  
         .add(FullScoringSucceededFeature, candidate.fullScoringSucceeded.getOrElse(false))
  
         .add(HasDisplayedTextFeature, hasDisplayedText)
  
  +      .add(InNetworkFeature, candidate.isInNetwork.getOrElse(true))
  
         .add(InReplyToTweetIdFeature, candidate.inReplyToTweetId)
  
         .add(IsAncestorCandidateFeature, candidate.isAncestorCandidate.getOrElse(false))
  
         .add(
  
