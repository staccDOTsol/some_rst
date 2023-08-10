a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/scorer/FeedbackFatigueScorer.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/scorer/FeedbackFatigueScorer.scala
==================================================

Change Range: -76,42 +76,56

Content:

::

index 98e7aeb64..ceb71139e 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/scorer/FeedbackFatigueScorer.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/scorer/FeedbackFatigueScorer.scala
  
     override def onlyIf(query: PipelineQuery): Boolean =
  
       query.features.exists(_.getOrElse(FeedbackHistoryFeature, Seq.empty).nonEmpty)
  
   
  
  -  private val DurationForFiltering = 14.days
  
  -  private val DurationForDiscounting = 140.days
  
  +  val DurationForFiltering = 14.days
  
  +  val DurationForDiscounting = 140.days
  
     private val ScoreMultiplierLowerBound = 0.2
  
     private val ScoreMultiplierUpperBound = 1.0
  
     private val ScoreMultiplierIncrementsCount = 4
  
           feedbackEntriesByEngagementType.getOrElse(tls.FeedbackEngagementType.Retweet, Seq.empty))
  
   
  
       val featureMaps = candidates.map { candidate =>
  
  +      val multiplier = getScoreMultiplier(
  
  +        candidate,
  
  +        authorsToDiscount,
  
  +        likersToDiscount,
  
  +        followersToDiscount,
  
  +        retweetersToDiscount
  
  +      )
  
         val score = candidate.features.getOrElse(ScoreFeature, None)
  
  -
  
  -      val originalAuthorId =
  
  -        CandidatesUtil.getOriginalAuthorId(candidate.features).getOrElse(0L)
  
  -      val originalAuthorMultiplier = authorsToDiscount.getOrElse(originalAuthorId, 1.0)
  
  -
  
  -      val likers = candidate.features.getOrElse(SGSValidLikedByUserIdsFeature, Seq.empty)
  
  -      val likerMultipliers = likers.flatMap(likersToDiscount.get)
  
  -      val likerMultiplier =
  
  -        if (likerMultipliers.nonEmpty && likers.size == likerMultipliers.size)
  
  -          likerMultipliers.max
  
  -        else 1.0
  
  -
  
  -      val followers = candidate.features.getOrElse(SGSValidFollowedByUserIdsFeature, Seq.empty)
  
  -      val followerMultipliers = followers.flatMap(followersToDiscount.get)
  
  -      val followerMultiplier =
  
  -        if (followerMultipliers.nonEmpty && followers.size == followerMultipliers.size)
  
  -          followerMultipliers.max
  
  -        else 1.0
  
  -
  
  -      val authorId = candidate.features.getOrElse(AuthorIdFeature, None).getOrElse(0L)
  
  -      val retweeterMultiplier =
  
  -        if (candidate.features.getOrElse(IsRetweetFeature, false))
  
  -          retweetersToDiscount.getOrElse(authorId, 1.0)
  
  -        else 1.0
  
  -
  
  -      val multiplier =
  
  -        originalAuthorMultiplier * likerMultiplier * followerMultiplier * retweeterMultiplier
  
  -
  
         FeatureMapBuilder().add(ScoreFeature, score.map(_ * multiplier)).build()
  
       }
  
   
  
       Stitch.value(featureMaps)
  
     }
  
   
  
  -  private def getUserDiscounts(
  
  +  def getScoreMultiplier(
  
  +    candidate: CandidateWithFeatures[TweetCandidate],
  
  +    authorsToDiscount: Map[Long, Double],
  
  +    likersToDiscount: Map[Long, Double],
  
  +    followersToDiscount: Map[Long, Double],
  
  +    retweetersToDiscount: Map[Long, Double],
  
  +  ): Double = {
  
  +    val originalAuthorId =
  
  +      CandidatesUtil.getOriginalAuthorId(candidate.features).getOrElse(0L)
  
  +    val originalAuthorMultiplier = authorsToDiscount.getOrElse(originalAuthorId, 1.0)
  
  +
  
  +    val likers = candidate.features.getOrElse(SGSValidLikedByUserIdsFeature, Seq.empty)
  
  +    val likerMultipliers = likers.flatMap(likersToDiscount.get)
  
  +    val likerMultiplier =
  
  +      if (likerMultipliers.nonEmpty && likers.size == likerMultipliers.size)
  
  +        likerMultipliers.max
  
  +      else 1.0
  
  +
  
  +    val followers = candidate.features.getOrElse(SGSValidFollowedByUserIdsFeature, Seq.empty)
  
  +    val followerMultipliers = followers.flatMap(followersToDiscount.get)
  
  +    val followerMultiplier =
  
  +      if (followerMultipliers.nonEmpty && followers.size == followerMultipliers.size &&
  
  +        likers.isEmpty)
  
  +        followerMultipliers.max
  
  +      else 1.0
  
  +
  
  +    val authorId = candidate.features.getOrElse(AuthorIdFeature, None).getOrElse(0L)
  
  +    val retweeterMultiplier =
  
  +      if (candidate.features.getOrElse(IsRetweetFeature, false))
  
  +        retweetersToDiscount.getOrElse(authorId, 1.0)
  
  +      else 1.0
  
  +
  
  +    originalAuthorMultiplier * likerMultiplier * followerMultiplier * retweeterMultiplier
  
  +  }
  
  +
  
  +  def getUserDiscounts(
  
       queryTime: Time,
  
       feedbackEntries: Seq[FeedbackEntry],
  
     ): Map[Long, Double] = {
  
