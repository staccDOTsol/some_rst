a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/filter/DropMaxCandidatesByScoreFilter.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/filter/DropMaxCandidatesByAggregatedScoreFilter.scala
==================================================

Change Range: -9,18 +9,26

Content:

::

similarity index 61%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/filter/DropMaxCandidatesByScoreFilter.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/filter/DropMaxCandidatesByAggregatedScoreFilter.scala
  
  index d75b301e3..a68c37450 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/filter/DropMaxCandidatesByScoreFilter.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/filter/DropMaxCandidatesByAggregatedScoreFilter.scala
  
   package com.twitter.home_mixer.product.list_recommended_users.filter
  
   
  
  -import com.twitter.home_mixer.product.list_recommended_users.model.ListFeatures.ScoreFeature
  
  +import com.twitter.home_mixer.product.list_recommended_users.model.ListRecommendedUsersFeatures.ScoreFeature
  
   import com.twitter.product_mixer.component_library.model.candidate.UserCandidate
  
   import com.twitter.product_mixer.core.functional_component.filter.Filter
  
   import com.twitter.product_mixer.core.functional_component.filter.FilterResult
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
   import com.twitter.stitch.Stitch
  
   
  
  -object DropMaxCandidatesByScoreFilter extends Filter[PipelineQuery, UserCandidate] {
  
  +object DropMaxCandidatesByAggregatedScoreFilter extends Filter[PipelineQuery, UserCandidate] {
  
   
  
  -  override val identifier: FilterIdentifier = FilterIdentifier("DropMaxCandidatesByScore")
  
  +  override val identifier: FilterIdentifier = FilterIdentifier("DropMaxCandidatesByAggregatedScore")
  
   
  
  -  private val MaxSimilarUserCandidates = 1000
  
  +  private val MaxSimilarUserCandidates = 150
  
   
  
     override def apply(
  
       query: PipelineQuery,
  
       candidates: Seq[CandidateWithFeatures[UserCandidate]]
  
     ): Stitch[FilterResult[UserCandidate]] = {
  
  -
  
  -    val sortedCandidates = candidates.sortBy(-_.features.getOrElse(ScoreFeature, 0.0))
  
  +    val userIdToAggregatedScoreMap = candidates
  
  +      .groupBy(_.candidate.id)
  
  +      .map {
  
  +        case (userId, candidates) =>
  
  +          val aggregatedScore = candidates.map(_.features.getOrElse(ScoreFeature, 0.0)).sum
  
  +          (userId, aggregatedScore)
  
  +      }
  
  +
  
  +    val sortedCandidates = candidates.sortBy(candidate =>
  
  +      -userIdToAggregatedScoreMap.getOrElse(candidate.candidate.id, 0.0))
  
   
  
       val (kept, removed) = sortedCandidates.map(_.candidate).splitAt(MaxSimilarUserCandidates)
  
   
  
