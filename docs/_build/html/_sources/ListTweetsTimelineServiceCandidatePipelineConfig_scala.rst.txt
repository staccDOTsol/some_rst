a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_tweets/ListTweetsTimelineServiceCandidatePipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_tweets/ListTweetsTimelineServiceCandidatePipelineConfig.scala
==================================================

Change Range: -51,4 +52,8

Content:

::

index aba9f47e0..1af95e873 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_tweets/ListTweetsTimelineServiceCandidatePipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_tweets/ListTweetsTimelineServiceCandidatePipelineConfig.scala
  
   import com.twitter.home_mixer.marshaller.timelines.TimelineServiceCursorMarshaller
  
   import com.twitter.home_mixer.product.list_tweets.model.ListTweetsQuery
  
   import com.twitter.home_mixer.product.list_tweets.param.ListTweetsParam.ServerMaxResultsParam
  
  +import com.twitter.home_mixer.service.HomeMixerAlertConfig
  
   import com.twitter.product_mixer.component_library.candidate_source.timeline_service.TimelineServiceTweetCandidateSource
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.functional_component.candidate_source.BaseCandidateSource
  
   
  
     override val featuresFromCandidateSourceTransformers: Seq[CandidateFeatureTransformer[t.Tweet]] =
  
       Seq(TimelineServiceResponseFeatureTransformer)
  
  +
  
  +  override val alerts = Seq(
  
  +    HomeMixerAlertConfig.BusinessHours.defaultSuccessRateAlert(99.7)
  
  +  )
  
   }
  
