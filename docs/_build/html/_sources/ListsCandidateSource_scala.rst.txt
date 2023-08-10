a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_source/ListsCandidateSource.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_source/ListsCandidateSource.scala
==================================================

Change Range: -0,0 +1,27

Content:

::

new file mode 100644
  
  index 000000000..395ac7da8
  
  --- /dev/null
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_source/ListsCandidateSource.scala
  
  +package com.twitter.home_mixer.product.scored_tweets.candidate_source
  
  +
  
  +import com.twitter.product_mixer.core.functional_component.candidate_source.CandidateSource
  
  +import com.twitter.product_mixer.core.model.common.identifier.CandidateSourceIdentifier
  
  +import com.twitter.stitch.Stitch
  
  +import com.twitter.stitch.timelineservice.TimelineService
  
  +import com.twitter.timelineservice.{thriftscala => tls}
  
  +
  
  +import javax.inject.Inject
  
  +import javax.inject.Singleton
  
  +
  
  +@Singleton
  
  +class ListsCandidateSource @Inject() (timelineService: TimelineService)
  
  +    extends CandidateSource[Seq[tls.TimelineQuery], tls.Tweet] {
  
  +
  
  +  override val identifier: CandidateSourceIdentifier = CandidateSourceIdentifier("Lists")
  
  +
  
  +  override def apply(requests: Seq[tls.TimelineQuery]): Stitch[Seq[tls.Tweet]] = {
  
  +    val timelines = Stitch.traverse(requests) { request => timelineService.getTimeline(request) }
  
  +
  
  +    timelines.map {
  
  +      _.flatMap {
  
  +        _.entries.collect { case tls.TimelineEntry.Tweet(tweet) => tweet }
  
  +      }
  
  +    }
  
  +  }
  
  +}
  
