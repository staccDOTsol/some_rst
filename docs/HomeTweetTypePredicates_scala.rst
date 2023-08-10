a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/HomeTweetTypePredicates.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/builder/HomeTweetTypePredicates.scala
==================================================

Change Range: -204,23 +206,50

Content:

::

similarity index 76%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/HomeTweetTypePredicates.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/builder/HomeTweetTypePredicates.scala
  
  index 0b06448d7..7272c360d 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/HomeTweetTypePredicates.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/builder/HomeTweetTypePredicates.scala
  
  -package com.twitter.home_mixer.functional_component.decorator
  
  +package com.twitter.home_mixer.functional_component.decorator.builder
  
   
  
   import com.twitter.conversions.DurationOps._
  
   import com.twitter.home_mixer.model.HomeFeatures._
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
  +import com.twitter.product_mixer.core.model.marshalling.response.urt.metadata.BasicTopicContextFunctionalityType
  
  +import com.twitter.product_mixer.core.model.marshalling.response.urt.metadata.RecommendationTopicContextFunctionalityType
  
   import com.twitter.timelinemixer.injection.model.candidate.SemanticCoreFeatures
  
   import com.twitter.tweetypie.{thriftscala => tpt}
  
   
  
       (
  
         "has_exclusive_conversation_author_id",
  
         _.getOrElse(ExclusiveConversationAuthorIdFeature, None).nonEmpty),
  
  -    ("is_eligible_for_connect_boost", _.getOrElse(AuthorIsEligibleForConnectBoostFeature, false)),
  
  +    ("is_eligible_for_connect_boost", _ => false),
  
       ("hashtag", _.getOrElse(EarlybirdFeature, None).exists(_.numHashtags > 0)),
  
       ("has_scheduled_space", _.getOrElse(AudioSpaceMetaDataFeature, None).exists(_.isScheduled)),
  
       ("has_recorded_space", _.getOrElse(AudioSpaceMetaDataFeature, None).exists(_.isRecorded)),
  
       ("is_read_from_cache", _.getOrElse(IsReadFromCacheFeature, false)),
  
  -    (
  
  -      "is_self_thread_tweet",
  
  -      _.getOrElse(ConversationFeature, None).exists(_.isSelfThreadTweet.contains(true))),
  
       ("get_initial", _.getOrElse(GetInitialFeature, false)),
  
       ("get_newer", _.getOrElse(GetNewerFeature, false)),
  
       ("get_middle", _.getOrElse(GetMiddleFeature, false)),
  
       ("get_older", _.getOrElse(GetOlderFeature, false)),
  
       ("pull_to_refresh", _.getOrElse(PullToRefreshFeature, false)),
  
       ("polling", _.getOrElse(PollingFeature, false)),
  
  -    ("tls_size_20_plus", _ => false),
  
  -    ("near_empty", _ => false),
  
  -    ("ranked_request", _ => false),
  
  +    ("near_empty", _.getOrElse(ServedSizeFeature, None).exists(_ < 3)),
  
  +    ("is_request_context_launch", _.getOrElse(IsLaunchRequestFeature, false)),
  
       ("mutual_follow", _.getOrElse(EarlybirdFeature, None).exists(_.fromMutualFollow)),
  
  +    (
  
  +      "less_than_10_mins_since_lnpt",
  
  +      _.getOrElse(LastNonPollingTimeFeature, None).exists(_.untilNow < 10.minutes)),
  
  +    ("served_in_conversation_module", _.getOrElse(ServedInConversationModuleFeature, false)),
  
       ("has_ticketed_space", _.getOrElse(AudioSpaceMetaDataFeature, None).exists(_.hasTickets)),
  
       ("in_utis_top5", _.getOrElse(PositionFeature, None).exists(_ < 5)),
  
  -    ("is_utis_pos0", _.getOrElse(PositionFeature, None).exists(_ == 0)),
  
  -    ("is_utis_pos1", _.getOrElse(PositionFeature, None).exists(_ == 1)),
  
  -    ("is_utis_pos2", _.getOrElse(PositionFeature, None).exists(_ == 2)),
  
  -    ("is_utis_pos3", _.getOrElse(PositionFeature, None).exists(_ == 3)),
  
  -    ("is_utis_pos4", _.getOrElse(PositionFeature, None).exists(_ == 4)),
  
       (
  
  -      "is_signup_request",
  
  -      candidate => candidate.getOrElse(AccountAgeFeature, None).exists(_.untilNow < 30.minutes)),
  
  -    ("empty_request", _ => false),
  
  -    ("served_size_less_than_5", _.getOrElse(ServedSizeFeature, None).exists(_ < 5)),
  
  -    ("served_size_less_than_10", _.getOrElse(ServedSizeFeature, None).exists(_ < 10)),
  
  -    ("served_size_less_than_20", _.getOrElse(ServedSizeFeature, None).exists(_ < 20)),
  
  +      "conversation_module_has_2_displayed_tweets",
  
  +      _.getOrElse(ConversationModule2DisplayedTweetsFeature, false)),
  
  +    ("empty_request", _.getOrElse(ServedSizeFeature, None).exists(_ == 0)),
  
       ("served_size_less_than_50", _.getOrElse(ServedSizeFeature, None).exists(_ < 50)),
  
       (
  
         "served_size_between_50_and_100",
  
         _.getOrElse(ServedSizeFeature, None).exists(size => size >= 50 && size < 100)),
  
       ("authored_by_contextual_user", _.getOrElse(AuthoredByContextualUserFeature, false)),
  
  +    (
  
  +      "is_self_thread_tweet",
  
  +      _.getOrElse(ConversationFeature, None).exists(_.isSelfThreadTweet.contains(true))),
  
       ("has_ancestors", _.getOrElse(AncestorsFeature, Seq.empty).nonEmpty),
  
       ("full_scoring_succeeded", _.getOrElse(FullScoringSucceededFeature, false)),
  
  +    ("served_size_less_than_20", _.getOrElse(ServedSizeFeature, None).exists(_ < 20)),
  
  +    ("served_size_less_than_10", _.getOrElse(ServedSizeFeature, None).exists(_ < 10)),
  
  +    ("served_size_less_than_5", _.getOrElse(ServedSizeFeature, None).exists(_ < 5)),
  
       (
  
         "account_age_less_than_30_minutes",
  
         _.getOrElse(AccountAgeFeature, None).exists(_.untilNow < 30.minutes)),
  
  -    (
  
  -      "account_age_less_than_1_day",
  
  -      _.getOrElse(AccountAgeFeature, None).exists(_.untilNow < 1.day)),
  
  -    (
  
  -      "account_age_less_than_7_days",
  
  -      _.getOrElse(AccountAgeFeature, None).exists(_.untilNow < 7.days)),
  
  +    ("conversation_module_has_gap", _.getOrElse(ConversationModuleHasGapFeature, false)),
  
       (
  
         "directed_at_user_is_in_first_degree",
  
         _.getOrElse(EarlybirdFeature, None).exists(_.directedAtUserIdIsInFirstDegree.contains(true))),
  
  -    ("root_user_is_in_first_degree", _ => false),
  
       (
  
         "has_semantic_core_annotation",
  
         _.getOrElse(EarlybirdFeature, None).exists(_.semanticCoreAnnotations.nonEmpty)),
  
       ("is_request_context_foreground", _.getOrElse(IsForegroundRequestFeature, false)),
  
  +    (
  
  +      "account_age_less_than_1_day",
  
  +      _.getOrElse(AccountAgeFeature, None).exists(_.untilNow < 1.day)),
  
  +    (
  
  +      "account_age_less_than_7_days",
  
  +      _.getOrElse(AccountAgeFeature, None).exists(_.untilNow < 7.days)),
  
       (
  
         "part_of_utt",
  
         _.getOrElse(EarlybirdFeature, None)
  
           .exists(_.semanticCoreAnnotations.exists(_.exists(annotation =>
  
             annotation.domainId == SemanticCoreFeatures.UnifiedTwitterTaxonomy)))),
  
  +    (
  
  +      "has_home_latest_request_past_week",
  
  +      _.getOrElse(FollowingLastNonPollingTimeFeature, None).exists(_.untilNow < 7.days)),
  
  +    ("is_utis_pos0", _.getOrElse(PositionFeature, None).exists(_ == 0)),
  
  +    ("is_utis_pos1", _.getOrElse(PositionFeature, None).exists(_ == 1)),
  
  +    ("is_utis_pos2", _.getOrElse(PositionFeature, None).exists(_ == 2)),
  
  +    ("is_utis_pos3", _.getOrElse(PositionFeature, None).exists(_ == 3)),
  
  +    ("is_utis_pos4", _.getOrElse(PositionFeature, None).exists(_ == 4)),
  
       ("is_random_tweet", _.getOrElse(IsRandomTweetFeature, false)),
  
       ("has_random_tweet_in_response", _.getOrElse(HasRandomTweetFeature, false)),
  
       ("is_random_tweet_above_in_utis", _.getOrElse(IsRandomTweetAboveFeature, false)),
  
  -    ("is_request_context_launch", _.getOrElse(IsLaunchRequestFeature, false)),
  
  -    ("viewer_is_employee", _ => false),
  
  -    ("viewer_is_timelines_employee", _ => false),
  
  -    ("viewer_follows_any_topics", _.getOrElse(UserFollowedTopicsCountFeature, None).exists(_ > 0)),
  
       (
  
         "has_ancestor_authored_by_viewer",
  
         candidate =>
  
             .getOrElse(AncestorsFeature, Seq.empty).exists(ancestor =>
  
               candidate.getOrElse(ViewerIdFeature, 0L) == ancestor.userId)),
  
       ("ancestor", _.getOrElse(IsAncestorCandidateFeature, false)),
  
  -    (
  
  -      "root_ancestor",
  
  -      candidate =>
  
  -        candidate.getOrElse(IsAncestorCandidateFeature, false) && candidate
  
  -          .getOrElse(InReplyToTweetIdFeature, None).isEmpty),
  
       (
  
         "deep_reply",
  
         candidate =>
  
         "tweet_age_less_than_15_seconds",
  
         _.getOrElse(OriginalTweetCreationTimeFromSnowflakeFeature, None)
  
           .exists(_.untilNow <= 15.seconds)),
  
  -    ("is_followed_topic_tweet", _ => false),
  
  -    ("is_recommended_topic_tweet", _ => false),
  
  -    ("is_topic_tweet", _ => false),
  
  -    ("preferred_language_matches_tweet_language", _ => false),
  
  +    (
  
  +      "less_than_1_hour_since_lnpt",
  
  +      _.getOrElse(LastNonPollingTimeFeature, None).exists(_.untilNow < 1.hour)),
  
  +    ("has_gte_10_favs", _.getOrElse(EarlybirdFeature, None).exists(_.favCountV2.exists(_ >= 10))),
  
       (
  
         "device_language_matches_tweet_language",
  
         candidate =>
  
           candidate.getOrElse(TweetLanguageFeature, None) ==
  
             candidate.getOrElse(DeviceLanguageFeature, None)),
  
  +    (
  
  +      "root_ancestor",
  
  +      candidate =>
  
  +        candidate.getOrElse(IsAncestorCandidateFeature, false) && candidate
  
  +          .getOrElse(InReplyToTweetIdFeature, None).isEmpty),
  
       ("question", _.getOrElse(EarlybirdFeature, None).exists(_.hasQuestion.contains(true))),
  
  -    ("in_network", _.getOrElse(FromInNetworkSourceFeature, true)),
  
  -    ("viewer_follows_original_author", _ => false),
  
  -    ("has_account_follow_prompt", _ => false),
  
  -    ("has_relevance_prompt", _ => false),
  
  -    ("has_topic_annotation_haug_prompt", _ => false),
  
  -    ("has_topic_annotation_random_precision_prompt", _ => false),
  
  -    ("has_topic_annotation_prompt", _ => false),
  
  +    ("in_network", _.getOrElse(InNetworkFeature, true)),
  
       (
  
         "has_political_annotation",
  
         _.getOrElse(EarlybirdFeature, None).exists(
  
         _.getOrElse(EarlybirdFeature, None)
  
           .exists(_.conversationControl.exists(_.isInstanceOf[tpt.ConversationControl.Community]))),
  
       ("has_zero_score", _.getOrElse(ScoreFeature, None).exists(_ == 0.0)),
  
  -    ("is_viewer_not_invited_to_reply", _ => false),
  
  -    ("is_viewer_invited_to_reply", _ => false),
  
  -    ("has_gte_10_favs", _.getOrElse(EarlybirdFeature, None).exists(_.favCountV2.exists(_ >= 10))),
  
  +    (
  
  +      "is_followed_topic_tweet",
  
  +      _.getOrElse(TopicContextFunctionalityTypeFeature, None)
  
  +        .exists(_ == BasicTopicContextFunctionalityType)),
  
  +    (
  
  +      "is_recommended_topic_tweet",
  
  +      _.getOrElse(TopicContextFunctionalityTypeFeature, None)
  
  +        .exists(_ == RecommendationTopicContextFunctionalityType)),
  
       ("has_gte_100_favs", _.getOrElse(EarlybirdFeature, None).exists(_.favCountV2.exists(_ >= 100))),
  
       ("has_gte_1k_favs", _.getOrElse(EarlybirdFeature, None).exists(_.favCountV2.exists(_ >= 1000))),
  
       (
  
       (
  
         "has_gte_100k_favs",
  
         _.getOrElse(EarlybirdFeature, None).exists(_.favCountV2.exists(_ >= 100000))),
  
  -    ("above_neighbor_is_topic_tweet", _ => false),
  
  -    ("is_topic_tweet_with_neighbor_below", _ => false),
  
       ("has_audio_space", _.getOrElse(AudioSpaceMetaDataFeature, None).exists(_.hasSpace)),
  
       ("has_live_audio_space", _.getOrElse(AudioSpaceMetaDataFeature, None).exists(_.isLive)),
  
       (
  
       (
  
         "has_toxicity_score_above_threshold",
  
         _.getOrElse(EarlybirdFeature, None).exists(_.toxicityScore.exists(_ > 0.91))),
  
  +    ("is_topic_tweet", _.getOrElse(TopicIdSocialContextFeature, None).isDefined),
  
       (
  
         "text_only",
  
         candidate =>
  
       ("has_3_images", _.getOrElse(NumImagesFeature, None).exists(_ == 3)),
  
       ("has_4_images", _.getOrElse(NumImagesFeature, None).exists(_ == 4)),
  
       ("has_card", _.getOrElse(EarlybirdFeature, None).exists(_.hasCard)),
  
  -    ("3_or_more_consecutive_not_in_network", _ => false),
  
  -    ("2_or_more_consecutive_not_in_network", _ => false),
  
  -    ("5_out_of_7_not_in_network", _ => false),
  
  -    ("7_out_of_7_not_in_network", _ => false),
  
  -    ("5_out_of_5_not_in_network", _ => false),
  
       ("user_follow_count_gte_50", _.getOrElse(UserFollowingCountFeature, None).exists(_ > 50)),
  
  -    ("has_liked_by_social_context", _ => false),
  
  -    ("has_followed_by_social_context", _ => false),
  
  -    ("has_topic_social_context", _ => false),
  
  -    ("timeline_entry_has_banner", _ => false),
  
  -    ("served_in_conversation_module", _.getOrElse(ServedInConversationModuleFeature, false)),
  
       (
  
  -      "conversation_module_has_2_displayed_tweets",
  
  -      _.getOrElse(ConversationModule2DisplayedTweetsFeature, false)),
  
  -    ("conversation_module_has_gap", _.getOrElse(ConversationModuleHasGapFeature, false)),
  
  -    ("served_in_recap_tweet_candidate_module_injection", _ => false),
  
  -    ("served_in_threaded_conversation_module", _ => false)
  
  +      "has_liked_by_social_context",
  
  +      candidateFeatures =>
  
  +        candidateFeatures
  
  +          .getOrElse(SGSValidLikedByUserIdsFeature, Seq.empty)
  
  +          .exists(candidateFeatures
  
  +            .getOrElse(PerspectiveFilteredLikedByUserIdsFeature, Seq.empty).toSet.contains)),
  
  +    (
  
  +      "has_followed_by_social_context",
  
  +      _.getOrElse(SGSValidFollowedByUserIdsFeature, Seq.empty).nonEmpty),
  
  +    (
  
  +      "has_topic_social_context",
  
  +      candidateFeatures =>
  
  +        candidateFeatures
  
  +          .getOrElse(TopicIdSocialContextFeature, None)
  
  +          .isDefined &&
  
  +          candidateFeatures.getOrElse(TopicContextFunctionalityTypeFeature, None).isDefined),
  
  +    ("video_lte_10_sec", _.getOrElse(VideoDurationMsFeature, None).exists(_ <= 10000)),
  
  +    (
  
  +      "video_bt_10_60_sec",
  
  +      _.getOrElse(VideoDurationMsFeature, None).exists(duration =>
  
  +        duration > 10000 && duration <= 60000)),
  
  +    ("video_gt_60_sec", _.getOrElse(VideoDurationMsFeature, None).exists(_ > 60000)),
  
  +    (
  
  +      "tweet_age_lte_30_minutes",
  
  +      _.getOrElse(OriginalTweetCreationTimeFromSnowflakeFeature, None)
  
  +        .exists(_.untilNow <= 30.minutes)),
  
  +    (
  
  +      "tweet_age_lte_1_hour",
  
  +      _.getOrElse(OriginalTweetCreationTimeFromSnowflakeFeature, None)
  
  +        .exists(_.untilNow <= 1.hour)),
  
  +    (
  
  +      "tweet_age_lte_6_hours",
  
  +      _.getOrElse(OriginalTweetCreationTimeFromSnowflakeFeature, None)
  
  +        .exists(_.untilNow <= 6.hours)),
  
  +    (
  
  +      "tweet_age_lte_12_hours",
  
  +      _.getOrElse(OriginalTweetCreationTimeFromSnowflakeFeature, None)
  
  +        .exists(_.untilNow <= 12.hours)),
  
  +    (
  
  +      "tweet_age_gte_24_hours",
  
  +      _.getOrElse(OriginalTweetCreationTimeFromSnowflakeFeature, None)
  
  +        .exists(_.untilNow >= 24.hours)),
  
     )
  
   
  
     val PredicateMap = CandidatePredicates.toMap
  
