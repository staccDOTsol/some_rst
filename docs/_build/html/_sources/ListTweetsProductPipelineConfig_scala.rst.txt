a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_tweets/ListTweetsProductPipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_tweets/ListTweetsProductPipelineConfig.scala
==================================================

Change Range: -90,5 +102,29

Content:

::

index 06afd9a92..b4b5b1924 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_tweets/ListTweetsProductPipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_tweets/ListTweetsProductPipelineConfig.scala
  
   package com.twitter.home_mixer.product.list_tweets
  
   
  
  +import com.twitter.conversions.DurationOps._
  
   import com.twitter.home_mixer.marshaller.timelines.ChronologicalCursorUnmarshaller
  
   import com.twitter.home_mixer.model.request.HomeMixerRequest
  
   import com.twitter.home_mixer.model.request.ListTweetsProduct
  
   import com.twitter.home_mixer.product.list_tweets.param.ListTweetsParam.ServerMaxResultsParam
  
   import com.twitter.home_mixer.product.list_tweets.param.ListTweetsParamConfig
  
   import com.twitter.home_mixer.service.HomeMixerAccessPolicy.DefaultHomeMixerAccessPolicy
  
  +import com.twitter.home_mixer.service.HomeMixerAlertConfig.DefaultNotificationGroup
  
   import com.twitter.product_mixer.component_library.model.cursor.UrtOrderedCursor
  
   import com.twitter.product_mixer.component_library.premarshaller.cursor.UrtCursorSerializer
  
   import com.twitter.product_mixer.core.functional_component.common.access_policy.AccessPolicy
  
  +import com.twitter.product_mixer.core.functional_component.common.alert.Alert
  
  +import com.twitter.product_mixer.core.functional_component.common.alert.EmptyResponseRateAlert
  
  +import com.twitter.product_mixer.core.functional_component.common.alert.LatencyAlert
  
  +import com.twitter.product_mixer.core.functional_component.common.alert.P99
  
  +import com.twitter.product_mixer.core.functional_component.common.alert.SuccessRateAlert
  
  +import com.twitter.product_mixer.core.functional_component.common.alert.ThroughputAlert
  
  +import com.twitter.product_mixer.core.functional_component.common.alert.predicate.TriggerIfAbove
  
  +import com.twitter.product_mixer.core.functional_component.common.alert.predicate.TriggerIfBelow
  
  +import com.twitter.product_mixer.core.functional_component.common.alert.predicate.TriggerIfLatencyAbove
  
   import com.twitter.product_mixer.core.model.common.identifier.ComponentIdentifier
  
   import com.twitter.product_mixer.core.model.common.identifier.ProductPipelineIdentifier
  
   import com.twitter.product_mixer.core.model.marshalling.request
  
     override val identifier: ProductPipelineIdentifier = ProductPipelineIdentifier("ListTweets")
  
     override val product: request.Product = ListTweetsProduct
  
     override val paramConfig: ProductParamConfig = listTweetsParamConfig
  
  +  override val denyLoggedOutUsers: Boolean = false
  
   
  
     override def pipelineQueryTransformer(
  
       request: HomeMixerRequest,
  
     override def pipelineSelector(query: ListTweetsQuery): ComponentIdentifier =
  
       listTweetsMixerPipelineConfig.identifier
  
   
  
  +  override val alerts: Seq[Alert] = Seq(
  
  +    SuccessRateAlert(
  
  +      notificationGroup = DefaultNotificationGroup,
  
  +      warnPredicate = TriggerIfBelow(99.9, 20, 30),
  
  +      criticalPredicate = TriggerIfBelow(99.9, 30, 30),
  
  +    ),
  
  +    LatencyAlert(
  
  +      notificationGroup = DefaultNotificationGroup,
  
  +      percentile = P99,
  
  +      warnPredicate = TriggerIfLatencyAbove(300.millis, 15, 30),
  
  +      criticalPredicate = TriggerIfLatencyAbove(400.millis, 15, 30)
  
  +    ),
  
  +    ThroughputAlert(
  
  +      notificationGroup = DefaultNotificationGroup,
  
  +      warnPredicate = TriggerIfAbove(3000),
  
  +      criticalPredicate = TriggerIfAbove(4000)
  
  +    ),
  
  +    EmptyResponseRateAlert(
  
  +      notificationGroup = DefaultNotificationGroup,
  
  +      warnPredicate = TriggerIfAbove(65),
  
  +      criticalPredicate = TriggerIfAbove(80)
  
  +    )
  
  +  )
  
  +
  
     override val debugAccessPolicies: Set[AccessPolicy] = DefaultHomeMixerAccessPolicy
  
   }
  
