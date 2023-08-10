a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/candidate_source/SimilarityBasedUsersCandidateSource.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/candidate_source/SimilarityBasedUsersCandidateSource.scala
==================================================

Change Range: -21,13 +21,19

Content:

::

similarity index 71%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/candidate_source/SimilarityBasedUsersCandidateSource.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/candidate_source/SimilarityBasedUsersCandidateSource.scala
  
  index f117e91f9..f5428d032 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/candidate_source/SimilarityBasedUsersCandidateSource.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/candidate_source/SimilarityBasedUsersCandidateSource.scala
  
  -package com.twitter.home_mixer.functional_component.candidate_source
  
  +package com.twitter.home_mixer.product.list_recommended_users.candidate_source
  
   
  
   import com.twitter.hermit.candidate.{thriftscala => t}
  
   import com.twitter.product_mixer.core.functional_component.candidate_source.CandidateSource
  
     private val fetcher: Fetcher[Long, Unit, t.Candidates] =
  
       similarUsersBySimsOnUserClientColumn.fetcher
  
   
  
  +  private val MaxCandidatesToKeep = 4000
  
  +
  
     override def apply(request: Seq[Long]): Stitch[Seq[t.Candidate]] = {
  
       Stitch
  
         .collect {
  
           request.map { userId =>
  
  -          fetcher.fetch(userId, Unit).map { result =>
  
  -            result.v.map(_.candidates).getOrElse(Seq.empty)
  
  -          }
  
  +          fetcher
  
  +            .fetch(userId, Unit).map { result =>
  
  +              result.v.map(_.candidates).getOrElse(Seq.empty)
  
  +            }.map { candidates =>
  
  +              val sortedCandidates = candidates.sortBy(-_.score)
  
  +              sortedCandidates.take(MaxCandidatesToKeep)
  
  +            }
  
           }
  
         }.map(_.flatten)
  
     }
  
