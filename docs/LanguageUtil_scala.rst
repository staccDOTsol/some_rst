a/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/LanguageUtil.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/LanguageUtil.scala
==================================================

Change Range: -25,7 +25,7

Content:

::

index 3969ef64d..23c77c27f 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/LanguageUtil.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/LanguageUtil.scala
  
   
  
   object LanguageUtil {
  
   
  
  -  private val DafaultMinProducedLanguageRatio = 0.05
  
  +  private val DefaultMinProducedLanguageRatio = 0.05
  
     private val DefaultMinConsumedLanguageConfidence = 0.8
  
   
  
     /**
  
      */
  
     def computeLanguages(
  
       userLanguages: smg.UserLanguages,
  
  -    minProducedLanguageRatio: Double = DafaultMinProducedLanguageRatio,
  
  +    minProducedLanguageRatio: Double = DefaultMinProducedLanguageRatio,
  
       minConsumedLanguageConfidence: Double = DefaultMinConsumedLanguageConfidence
  
     ): Seq[scc.ThriftLanguage] = {
  
       val languageConfidenceMap = computeLanguageConfidenceMap(
  
         minProducedLanguageRatio,
  
         minConsumedLanguageConfidence
  
       )
  
  -    languageConfidenceMap.toSeq.sortWith(_._2 > _._2).map(_._1) // _1 = language, _2 = score
  
  +    languageConfidenceMap.toSeq.sortBy(-_._2).map(_._1) // _1 = language, _2 = score
  
     }
  
   
  
     /**
  
