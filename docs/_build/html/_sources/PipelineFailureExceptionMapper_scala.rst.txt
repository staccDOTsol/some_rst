a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/PipelineFailureExceptionMapper.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/PipelineFailureExceptionMapper.scala
==================================================

Change Range: -2,7 +2,7

Content:

::

index 65f0ac7bd..ad15988cb 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/PipelineFailureExceptionMapper.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/PipelineFailureExceptionMapper.scala
  
   
  
   import com.twitter.finatra.thrift.exceptions.ExceptionMapper
  
   import com.twitter.home_mixer.{thriftscala => t}
  
  -import com.twitter.inject.Logging
  
  +import com.twitter.util.logging.Logging
  
   import com.twitter.product_mixer.core.pipeline.pipeline_failure.PipelineFailure
  
   import com.twitter.product_mixer.core.pipeline.pipeline_failure.ProductDisabled
  
   import com.twitter.scrooge.ThriftException
  
