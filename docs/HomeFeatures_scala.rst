a/home-mixer/server/src/main/scala/com/twitter/home_mixer/model/HomeFeatures.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/model/HomeFeatures.scala
==================================================

Change Range: -223,6 +266,26

Content:

::

index 032623cd4..fe085b1f9 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/model/HomeFeatures.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/model/HomeFeatures.scala
  
   
  
   import com.twitter.core_workflows.user_model.{thriftscala => um}
  
   import com.twitter.dal.personal_data.{thriftjava => pd}
  
  -import com.twitter.escherbird.{thriftscala => esb}
  
   import com.twitter.gizmoduck.{thriftscala => gt}
  
   import com.twitter.home_mixer.{thriftscala => hmt}
  
   import com.twitter.ml.api.constant.SharedFeatures
  
   import com.twitter.product_mixer.core.feature.FeatureWithDefaultOnFailure
  
   import com.twitter.product_mixer.core.feature.datarecord.BoolDataRecordCompatible
  
   import com.twitter.product_mixer.core.feature.datarecord.DataRecordFeature
  
  +import com.twitter.product_mixer.core.feature.datarecord.DataRecordOptionalFeature
  
  +import com.twitter.product_mixer.core.feature.datarecord.DoubleDataRecordCompatible
  
   import com.twitter.product_mixer.core.feature.datarecord.LongDiscreteDataRecordCompatible
  
   import com.twitter.product_mixer.core.model.marshalling.response.urt.metadata.TopicContextFunctionalityType
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
   import com.twitter.search.common.features.{thriftscala => sc}
  
  +import com.twitter.search.earlybird.{thriftscala => eb}
  
   import com.twitter.timelinemixer.clients.manhattan.DismissInfo
  
   import com.twitter.timelinemixer.clients.persistence.TimelineResponseV3
  
   import com.twitter.timelinemixer.injection.model.candidate.AudioSpaceMetaData
  
   import com.twitter.timelines.impressionbloomfilter.{thriftscala => blm}
  
   import com.twitter.timelines.model.UserId
  
   import com.twitter.timelines.prediction.features.common.TimelinesSharedFeatures
  
  +import com.twitter.timelines.prediction.features.engagement_features.EngagementDataRecordFeatures
  
   import com.twitter.timelines.prediction.features.recap.RecapFeatures
  
   import com.twitter.timelines.prediction.features.request_context.RequestContextFeatures
  
   import com.twitter.timelines.service.{thriftscala => tst}
  
      * who created the Tweet that was retweeted.
  
      */
  
     object AuthorIdFeature
  
  -      extends Feature[TweetCandidate, Option[Long]]
  
  +      extends DataRecordOptionalFeature[TweetCandidate, Long]
  
         with LongDiscreteDataRecordCompatible {
  
       override val featureName: String = SharedFeatures.AUTHOR_ID.getFeatureName
  
       override val personalDataTypes: Set[pd.PersonalDataType] = Set(pd.PersonalDataType.UserId)
  
     }
  
  -  object AuthorIsEligibleForConnectBoostFeature extends Feature[TweetCandidate, Boolean]
  
  +
  
     object AuthorIsBlueVerifiedFeature extends Feature[TweetCandidate, Boolean]
  
  +  object AuthorIsGoldVerifiedFeature extends Feature[TweetCandidate, Boolean]
  
  +  object AuthorIsGrayVerifiedFeature extends Feature[TweetCandidate, Boolean]
  
  +  object AuthorIsLegacyVerifiedFeature extends Feature[TweetCandidate, Boolean]
  
  +  object AuthorIsCreatorFeature extends Feature[TweetCandidate, Boolean]
  
  +  object AuthorIsProtectedFeature extends Feature[TweetCandidate, Boolean]
  
  +
  
     object AuthoredByContextualUserFeature extends Feature[TweetCandidate, Boolean]
  
     object CachedCandidatePipelineIdentifierFeature extends Feature[TweetCandidate, Option[String]]
  
     object CandidateSourceIdFeature
  
     object DirectedAtUserIdFeature extends Feature[TweetCandidate, Option[Long]]
  
     object EarlybirdFeature extends Feature[TweetCandidate, Option[sc.ThriftTweetFeatures]]
  
     object EarlybirdScoreFeature extends Feature[TweetCandidate, Option[Double]]
  
  +  object EarlybirdSearchResultFeature extends Feature[TweetCandidate, Option[eb.ThriftSearchResult]]
  
     object EntityTokenFeature extends Feature[TweetCandidate, Option[String]]
  
     object ExclusiveConversationAuthorIdFeature extends Feature[TweetCandidate, Option[Long]]
  
  +  object FavoritedByCountFeature
  
  +      extends DataRecordFeature[TweetCandidate, Double]
  
  +      with DoubleDataRecordCompatible {
  
  +    override val featureName: String =
  
  +      EngagementDataRecordFeatures.InNetworkFavoritesCount.getFeatureName
  
  +    override val personalDataTypes: Set[pd.PersonalDataType] =
  
  +      Set(pd.PersonalDataType.CountOfPrivateLikes, pd.PersonalDataType.CountOfPublicLikes)
  
  +  }
  
     object FavoritedByUserIdsFeature extends Feature[TweetCandidate, Seq[Long]]
  
     object FeedbackHistoryFeature extends Feature[TweetCandidate, Seq[FeedbackEntry]]
  
  +  object RetweetedByCountFeature
  
  +      extends DataRecordFeature[TweetCandidate, Double]
  
  +      with DoubleDataRecordCompatible {
  
  +    override val featureName: String =
  
  +      EngagementDataRecordFeatures.InNetworkRetweetsCount.getFeatureName
  
  +    override val personalDataTypes: Set[pd.PersonalDataType] =
  
  +      Set(pd.PersonalDataType.CountOfPrivateRetweets, pd.PersonalDataType.CountOfPublicRetweets)
  
  +  }
  
     object RetweetedByEngagerIdsFeature extends Feature[TweetCandidate, Seq[Long]]
  
  +  object RepliedByCountFeature
  
  +      extends DataRecordFeature[TweetCandidate, Double]
  
  +      with DoubleDataRecordCompatible {
  
  +    override val featureName: String =
  
  +      EngagementDataRecordFeatures.InNetworkRepliesCount.getFeatureName
  
  +    override val personalDataTypes: Set[pd.PersonalDataType] =
  
  +      Set(pd.PersonalDataType.CountOfPrivateReplies, pd.PersonalDataType.CountOfPublicReplies)
  
  +  }
  
     object RepliedByEngagerIdsFeature extends Feature[TweetCandidate, Seq[Long]]
  
     object FollowedByUserIdsFeature extends Feature[TweetCandidate, Seq[Long]]
  
   
  
       override val personalDataTypes: Set[pd.PersonalDataType] = Set.empty
  
     }
  
     object IsRandomTweetFeature
  
  -      extends Feature[TweetCandidate, Boolean]
  
  +      extends DataRecordFeature[TweetCandidate, Boolean]
  
         with BoolDataRecordCompatible {
  
       override val featureName: String = TimelinesSharedFeatures.IS_RANDOM_TWEET.getFeatureName
  
       override val personalDataTypes: Set[pd.PersonalDataType] = Set.empty
  
     object IsReadFromCacheFeature extends Feature[TweetCandidate, Boolean]
  
     object IsRetweetFeature extends Feature[TweetCandidate, Boolean]
  
     object IsRetweetedReplyFeature extends Feature[TweetCandidate, Boolean]
  
  +  object IsSupportAccountReplyFeature extends Feature[TweetCandidate, Boolean]
  
     object LastScoredTimestampMsFeature extends Feature[TweetCandidate, Option[Long]]
  
     object NonSelfFavoritedByUserIdsFeature extends Feature[TweetCandidate, Seq[Long]]
  
     object NumImagesFeature extends Feature[TweetCandidate, Option[Int]]
  
         extends Feature[TweetCandidate, Map[String, Double]]
  
     object SocialContextFeature extends Feature[TweetCandidate, Option[tst.SocialContext]]
  
     object SourceTweetIdFeature
  
  -      extends Feature[TweetCandidate, Option[Long]]
  
  +      extends DataRecordOptionalFeature[TweetCandidate, Long]
  
         with LongDiscreteDataRecordCompatible {
  
       override val featureName: String = TimelinesSharedFeatures.SOURCE_TWEET_ID.getFeatureName
  
       override val personalDataTypes: Set[pd.PersonalDataType] = Set(pd.PersonalDataType.TweetId)
  
     object TweetUrlsFeature extends Feature[TweetCandidate, Seq[String]]
  
     object VideoDurationMsFeature extends Feature[TweetCandidate, Option[Int]]
  
     object ViewerIdFeature
  
  -      extends Feature[TweetCandidate, Long]
  
  +      extends DataRecordFeature[TweetCandidate, Long]
  
         with LongDiscreteDataRecordCompatible {
  
       override def featureName: String = SharedFeatures.USER_ID.getFeatureName
  
       override def personalDataTypes: Set[pd.PersonalDataType] = Set(pd.PersonalDataType.UserId)
  
     object WeightedModelScoreFeature extends Feature[TweetCandidate, Option[Double]]
  
     object MentionUserIdFeature extends Feature[TweetCandidate, Seq[Long]]
  
     object MentionScreenNameFeature extends Feature[TweetCandidate, Seq[String]]
  
  -  object SemanticAnnotationFeature extends Feature[TweetCandidate, Seq[esb.TweetEntityAnnotation]]
  
     object HasImageFeature extends Feature[TweetCandidate, Boolean]
  
     object HasVideoFeature extends Feature[TweetCandidate, Boolean]
  
   
  
     // Tweetypie VF Features
  
  -  object IsHydratedFeature extends FeatureWithDefaultOnFailure[TweetCandidate, Boolean] {
  
  -    override val defaultValue: Boolean = true
  
  -  }
  
  +  object IsHydratedFeature extends Feature[TweetCandidate, Boolean]
  
     object IsNsfwFeature extends Feature[TweetCandidate, Boolean]
  
     object QuotedTweetDroppedFeature extends Feature[TweetCandidate, Boolean]
  
     // Raw Tweet Text from Tweetypie
  
     object TweetTextFeature extends Feature[TweetCandidate, Option[String]]
  
   
  
  +  object AuthorEnabledPreviewsFeature extends Feature[TweetCandidate, Boolean]
  
  +  object IsTweetPreviewFeature extends Feature[TweetCandidate, Boolean]
  
  +
  
     // SGS Features
  
     /**
  
      * By convention, this is set to true for retweets of non-followed authors
  
     // Query Features
  
     object AccountAgeFeature extends Feature[PipelineQuery, Option[Time]]
  
     object ClientIdFeature
  
  -      extends Feature[PipelineQuery, Option[Long]]
  
  +      extends DataRecordOptionalFeature[PipelineQuery, Long]
  
         with LongDiscreteDataRecordCompatible {
  
       override def featureName: String = SharedFeatures.CLIENT_ID.getFeatureName
  
       override def personalDataTypes: Set[pd.PersonalDataType] = Set(pd.PersonalDataType.ClientType)
  
     }
  
  -  object CachedScoredTweetsFeature extends Feature[PipelineQuery, Seq[hmt.CachedScoredTweet]]
  
  +  object CachedScoredTweetsFeature extends Feature[PipelineQuery, Seq[hmt.ScoredTweet]]
  
     object DeviceLanguageFeature extends Feature[PipelineQuery, Option[String]]
  
     object DismissInfoFeature
  
         extends FeatureWithDefaultOnFailure[PipelineQuery, Map[st.SuggestType, Option[DismissInfo]]] {
  
       override def defaultValue: Map[st.SuggestType, Option[DismissInfo]] = Map.empty
  
     }
  
     object FollowingLastNonPollingTimeFeature extends Feature[PipelineQuery, Option[Time]]
  
  -  object GetInitialFeature extends Feature[PipelineQuery, Boolean] with BoolDataRecordCompatible {
  
  +  object GetInitialFeature
  
  +      extends DataRecordFeature[PipelineQuery, Boolean]
  
  +      with BoolDataRecordCompatible {
  
       override def featureName: String = RequestContextFeatures.IS_GET_INITIAL.getFeatureName
  
       override def personalDataTypes: Set[pd.PersonalDataType] = Set.empty
  
     }
  
  -  object GetMiddleFeature extends Feature[PipelineQuery, Boolean] with BoolDataRecordCompatible {
  
  +  object GetMiddleFeature
  
  +      extends DataRecordFeature[PipelineQuery, Boolean]
  
  +      with BoolDataRecordCompatible {
  
       override def featureName: String = RequestContextFeatures.IS_GET_MIDDLE.getFeatureName
  
       override def personalDataTypes: Set[pd.PersonalDataType] = Set.empty
  
     }
  
  -  object GetNewerFeature extends Feature[PipelineQuery, Boolean] with BoolDataRecordCompatible {
  
  +  object GetNewerFeature
  
  +      extends DataRecordFeature[PipelineQuery, Boolean]
  
  +      with BoolDataRecordCompatible {
  
       override def featureName: String = RequestContextFeatures.IS_GET_NEWER.getFeatureName
  
       override def personalDataTypes: Set[pd.PersonalDataType] = Set.empty
  
     }
  
  -  object GetOlderFeature extends Feature[PipelineQuery, Boolean] with BoolDataRecordCompatible {
  
  +  object GetOlderFeature
  
  +      extends DataRecordFeature[PipelineQuery, Boolean]
  
  +      with BoolDataRecordCompatible {
  
       override def featureName: String = RequestContextFeatures.IS_GET_OLDER.getFeatureName
  
       override def personalDataTypes: Set[pd.PersonalDataType] = Set.empty
  
     }
  
     object GuestIdFeature
  
  -      extends Feature[PipelineQuery, Option[Long]]
  
  +      extends DataRecordOptionalFeature[PipelineQuery, Long]
  
         with LongDiscreteDataRecordCompatible {
  
       override def featureName: String = SharedFeatures.GUEST_ID.getFeatureName
  
       override def personalDataTypes: Set[pd.PersonalDataType] = Set(pd.PersonalDataType.GuestId)
  
     }
  
  -  object HasDarkRequestFeature extends Feature[TweetCandidate, Option[Boolean]]
  
  +  object HasDarkRequestFeature extends Feature[PipelineQuery, Option[Boolean]]
  
     object ImpressionBloomFilterFeature
  
         extends FeatureWithDefaultOnFailure[PipelineQuery, blm.ImpressionBloomFilterSeq] {
  
       override def defaultValue: blm.ImpressionBloomFilterSeq =
  
     // Internal id generated per request, mainly to deduplicate re-served cached tweets in logging
  
     object ServedRequestIdFeature extends Feature[PipelineQuery, Option[Long]]
  
     object ServedTweetIdsFeature extends Feature[PipelineQuery, Seq[Long]]
  
  +  object ServedTweetPreviewIdsFeature extends Feature[PipelineQuery, Seq[Long]]
  
  +  object TimelineServiceTweetsFeature extends Feature[PipelineQuery, Seq[Long]]
  
  +  object TimestampFeature
  
  +      extends DataRecordFeature[PipelineQuery, Long]
  
  +      with LongDiscreteDataRecordCompatible {
  
  +    override def featureName: String = SharedFeatures.TIMESTAMP.getFeatureName
  
  +    override def personalDataTypes: Set[pd.PersonalDataType] = Set.empty
  
  +  }
  
  +  object TimestampGMTDowFeature
  
  +      extends DataRecordFeature[PipelineQuery, Long]
  
  +      with LongDiscreteDataRecordCompatible {
  
  +    override def featureName: String = RequestContextFeatures.TIMESTAMP_GMT_DOW.getFeatureName
  
  +    override def personalDataTypes: Set[pd.PersonalDataType] = Set.empty
  
  +  }
  
  +  object TimestampGMTHourFeature
  
  +      extends DataRecordFeature[PipelineQuery, Long]
  
  +      with LongDiscreteDataRecordCompatible {
  
  +    override def featureName: String = RequestContextFeatures.TIMESTAMP_GMT_HOUR.getFeatureName
  
  +    override def personalDataTypes: Set[pd.PersonalDataType] = Set.empty
  
  +  }
  
     object TweetImpressionsFeature extends Feature[PipelineQuery, Seq[imp.TweetImpressionsEntry]]
  
     object UserFollowedTopicsCountFeature extends Feature[PipelineQuery, Option[Int]]
  
     object UserFollowingCountFeature extends Feature[PipelineQuery, Option[Int]]
  
