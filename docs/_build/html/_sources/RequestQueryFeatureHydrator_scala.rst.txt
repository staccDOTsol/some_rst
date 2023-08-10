a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RequestQueryFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RequestQueryFeatureHydrator.scala
==================================================

Change Range: -97,8 +105,15

Content:

::

index bda746816..3968c9532 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RequestQueryFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RequestQueryFeatureHydrator.scala
  
   import com.twitter.product_mixer.core.model.marshalling.response.urt.operation.TopCursor
  
   import com.twitter.product_mixer.core.pipeline.HasPipelineCursor
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  +import com.twitter.product_mixer.core.pipeline.pipeline_failure.BadRequest
  
  +import com.twitter.product_mixer.core.pipeline.pipeline_failure.PipelineFailure
  
   import com.twitter.search.common.util.lang.ThriftLanguageUtil
  
   import com.twitter.snowflake.id.SnowflakeId
  
   import com.twitter.stitch.Stitch
  
  +import com.twitter.timelines.prediction.adapters.request_context.RequestContextAdapter.dowFromTimestamp
  
  +import com.twitter.timelines.prediction.adapters.request_context.RequestContextAdapter.hourFromTimestamp
  
   import java.util.UUID
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
       PullToRefreshFeature,
  
       RequestJoinIdFeature,
  
       ServedRequestIdFeature,
  
  +    TimestampFeature,
  
  +    TimestampGMTDowFeature,
  
  +    TimestampGMTHourFeature,
  
       ViewerIdFeature
  
     )
  
   
  
     override def hydrate(query: Query): Stitch[FeatureMap] = {
  
       val requestContext = query.deviceContext.flatMap(_.requestContextValue)
  
       val servedRequestId = UUID.randomUUID.getMostSignificantBits
  
  +    val timestamp = query.queryTime.inMilliseconds
  
   
  
       val featureMap = FeatureMapBuilder()
  
         .add(AccountAgeFeature, query.getOptionalUserId.flatMap(SnowflakeId.timeFromIdOpt))
  
         .add(PullToRefreshFeature, requestContext.contains(RequestContext.PullToRefresh))
  
         .add(ServedRequestIdFeature, Some(servedRequestId))
  
         .add(RequestJoinIdFeature, getRequestJoinId(servedRequestId))
  
  +      .add(TimestampFeature, timestamp)
  
  +      .add(TimestampGMTDowFeature, dowFromTimestamp(timestamp))
  
  +      .add(TimestampGMTHourFeature, hourFromTimestamp(timestamp))
  
         .add(HasDarkRequestFeature, hasDarkRequest)
  
  -      .add(ViewerIdFeature, query.getRequiredUserId)
  
  +      .add(
  
  +        ViewerIdFeature,
  
  +        query.getOptionalUserId
  
  +          .orElse(query.getGuestId).getOrElse(
  
  +            throw PipelineFailure(BadRequest, "Missing viewer id")))
  
         .build()
  
   
  
       Stitch.value(featureMap)
  
