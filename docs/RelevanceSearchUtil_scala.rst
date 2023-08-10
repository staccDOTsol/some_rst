a/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/earlybird/RelevanceSearchUtil.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/earlybird/RelevanceSearchUtil.scala
==================================================

Change Range: -39,7 +28,7

Content:

::

index 0de4546a6..40b727141 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/earlybird/RelevanceSearchUtil.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/earlybird/RelevanceSearchUtil.scala
  
   package com.twitter.home_mixer.util.earlybird
  
   
  
   import com.twitter.search.common.schema.earlybird.EarlybirdFieldConstants.EarlybirdFieldConstant
  
  -import com.twitter.search.common.ranking.{thriftscala => scr}
  
   import com.twitter.search.earlybird.{thriftscala => eb}
  
   
  
   object RelevanceSearchUtil {
  
     val Hashtags: String = EarlybirdFieldConstant.HASHTAGS_FACET
  
     val FacetsToFetch: Seq[String] = Seq(Mentions, Hashtags)
  
   
  
  -  private val RankingParams: scr.ThriftRankingParams = {
  
  -    scr.ThriftRankingParams(
  
  -      `type` = Some(scr.ThriftScoringFunctionType.TensorflowBased),
  
  -      selectedTensorflowModel = Some("timelines_rectweet_replica"),
  
  -      minScore = -1.0e100,
  
  -      selectedModels = Some(Map("home_mixer_unified_engagement_prod" -> 1.0)),
  
  -      applyBoosts = false,
  
  -    )
  
  -  }
  
  -
  
     val MetadataOptions: eb.ThriftSearchResultMetadataOptions = {
  
       eb.ThriftSearchResultMetadataOptions(
  
         getTweetUrls = true,
  
       eb.ThriftSearchRelevanceOptions(
  
         proximityScoring = true,
  
         maxConsecutiveSameUser = Some(2),
  
  -      rankingParams = Some(RankingParams),
  
  +      rankingParams = None,
  
         maxHitsToProcess = Some(500),
  
         maxUserBlendCount = Some(3),
  
         proximityPhraseWeight = 9.0,
  
