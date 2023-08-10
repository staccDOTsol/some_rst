a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/filter/KeepBestOutOfNetworkTweetPerAuthorFilter.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/filter/PreviouslyServedTweetPreviewsFilter.scala
==================================================

Change Range: -11,24 +9,20

Content:

::

similarity index 55%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/filter/KeepBestOutOfNetworkTweetPerAuthorFilter.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/filter/PreviouslyServedTweetPreviewsFilter.scala
  
  index 87c324b89..0ff820479 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/filter/KeepBestOutOfNetworkTweetPerAuthorFilter.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/filter/PreviouslyServedTweetPreviewsFilter.scala
  
   package com.twitter.home_mixer.functional_component.filter
  
   
  
  -import com.twitter.home_mixer.model.HomeFeatures.AuthorIdFeature
  
  -import com.twitter.home_mixer.model.HomeFeatures.InNetworkFeature
  
  -import com.twitter.home_mixer.model.HomeFeatures.ScoreFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.ServedTweetPreviewIdsFeature
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.functional_component.filter.Filter
  
   import com.twitter.product_mixer.core.functional_component.filter.FilterResult
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
   import com.twitter.stitch.Stitch
  
   
  
  -object KeepBestOutOfNetworkTweetPerAuthorFilter extends Filter[PipelineQuery, TweetCandidate] {
  
  +object PreviouslyServedTweetPreviewsFilter extends Filter[PipelineQuery, TweetCandidate] {
  
   
  
  -  override val identifier: FilterIdentifier = FilterIdentifier("KeepBestOutOfNetworkTweetPerAuthor")
  
  +  override val identifier: FilterIdentifier = FilterIdentifier("PreviouslyServedTweetPreviews")
  
   
  
     override def apply(
  
       query: PipelineQuery,
  
       candidates: Seq[CandidateWithFeatures[TweetCandidate]]
  
     ): Stitch[FilterResult[TweetCandidate]] = {
  
  -    // Set containing best OON tweet for each authorId
  
  -    val bestCandidatesForAuthorId = candidates
  
  -      .filter(!_.features.getOrElse(InNetworkFeature, true))
  
  -      .groupBy(_.features.getOrElse(AuthorIdFeature, None))
  
  -      .values.map(_.maxBy(_.features.getOrElse(ScoreFeature, None)))
  
  -      .toSet
  
  +
  
  +    val servedTweetPreviewIds =
  
  +      query.features.map(_.getOrElse(ServedTweetPreviewIdsFeature, Seq.empty)).toSeq.flatten.toSet
  
   
  
       val (removed, kept) = candidates.partition { candidate =>
  
  -      !candidate.features.getOrElse(InNetworkFeature, true) &&
  
  -      !bestCandidatesForAuthorId.contains(candidate)
  
  +      servedTweetPreviewIds.contains(candidate.candidate.id)
  
       }
  
   
  
       Stitch.value(FilterResult(kept = kept.map(_.candidate), removed = removed.map(_.candidate)))
  
