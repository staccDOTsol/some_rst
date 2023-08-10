a/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/CandidatesUtil.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/CandidatesUtil.scala
==================================================

Change Range: -52,11 +52,22

Content:

::

index 4bf265fd1..06fe5646c 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/CandidatesUtil.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/CandidatesUtil.scala
  
   import com.twitter.home_mixer.model.HomeFeatures.AuthorIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.FavoritedByUserIdsFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.HasImageFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.InReplyToTweetIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.IsRetweetFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.MediaUnderstandingAnnotationIdsFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.RepliedByEngagerIdsFeature
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
   import com.twitter.product_mixer.core.pipeline.pipeline_failure.PipelineFailure
  
   import com.twitter.product_mixer.core.pipeline.pipeline_failure.UnexpectedCandidateResult
  
  -
  
   import scala.reflect.ClassTag
  
   
  
   object CandidatesUtil {
  
       case _ => false
  
     }
  
   
  
  +  def getOriginalTweetId(candidate: CandidateWithFeatures[TweetCandidate]): Long = {
  
  +    if (candidate.features.getOrElse(IsRetweetFeature, false))
  
  +      candidate.features.getOrElse(SourceTweetIdFeature, None).getOrElse(candidate.candidate.id)
  
  +    else
  
  +      candidate.candidate.id
  
  +  }
  
  +
  
     def getOriginalAuthorId(candidateFeatures: FeatureMap): Option[Long] =
  
       if (candidateFeatures.getOrElse(IsRetweetFeature, false))
  
         candidateFeatures.getOrElse(SourceUserIdFeature, None)
  
       else candidateFeatures.getOrElse(AuthorIdFeature, None)
  
   
  
  +  def isOriginalTweet(candidate: CandidateWithFeatures[TweetCandidate]): Boolean =
  
  +    !candidate.features.getOrElse(IsRetweetFeature, false) &&
  
  +      candidate.features.getOrElse(InReplyToTweetIdFeature, None).isEmpty
  
  +
  
     def getEngagerUserIds(
  
       candidateFeatures: FeatureMap
  
     ): Seq[Long] = {
  
