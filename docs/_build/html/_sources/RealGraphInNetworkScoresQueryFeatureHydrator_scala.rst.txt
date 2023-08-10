a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RealGraphInNetworkScoresQueryFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RealGraphInNetworkScoresQueryFeatureHydrator.scala
==================================================

Change Range: -32,11 +32,15

Content:

::

index f04490625..92ab90f2c 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RealGraphInNetworkScoresQueryFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RealGraphInNetworkScoresQueryFeatureHydrator.scala
  
         val realGraphScoresFeatures = realGraphFollowedUsers
  
           .getOrElse(Seq.empty)
  
           .sortBy(-_.score)
  
  -        .map(candidate => candidate.userId -> candidate.score)
  
  +        .map(candidate => candidate.userId -> scaleScore(candidate.score))
  
           .take(RealGraphCandidateCount)
  
           .toMap
  
   
  
         FeatureMapBuilder().add(RealGraphInNetworkScoresFeature, realGraphScoresFeatures).build()
  
       }
  
     }
  
  +
  
  +  // Rescale Real Graph v2 scores from [0,1] to the v1 scores distribution [1,2.97]
  
  +  private def scaleScore(score: Double): Double =
  
  +    if (score >= 0.0 && score <= 1.0) score * 1.97 + 1.0 else score
  
   }
  
