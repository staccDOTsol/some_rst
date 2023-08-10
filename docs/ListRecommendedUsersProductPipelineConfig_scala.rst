a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/ListRecommendedUsersProductPipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/ListRecommendedUsersProductPipelineConfig.scala
==================================================

Change Range: -75,5 +84,19

Content:

::

index e20bb8aec..f0c2ac70e 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/ListRecommendedUsersProductPipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/ListRecommendedUsersProductPipelineConfig.scala
  
   package com.twitter.home_mixer.product.list_recommended_users
  
   
  
  +import com.twitter.conversions.DurationOps._
  
   import com.twitter.home_mixer.marshaller.timelines.RecommendedUsersCursorUnmarshaller
  
   import com.twitter.home_mixer.model.request.HomeMixerRequest
  
   import com.twitter.home_mixer.model.request.ListRecommendedUsersProduct
  
   import com.twitter.home_mixer.product.list_recommended_users.param.ListRecommendedUsersParam.ServerMaxResultsParam
  
   import com.twitter.home_mixer.product.list_recommended_users.param.ListRecommendedUsersParamConfig
  
   import com.twitter.home_mixer.service.HomeMixerAccessPolicy.DefaultHomeMixerAccessPolicy
  
  +import com.twitter.home_mixer.service.HomeMixerAlertConfig.DefaultNotificationGroup
  
   import com.twitter.product_mixer.component_library.premarshaller.cursor.UrtCursorSerializer
  
   import com.twitter.product_mixer.core.functional_component.common.access_policy.AccessPolicy
  
  +import com.twitter.product_mixer.core.functional_component.common.alert.predicate.TriggerIfBelow
  
  +import com.twitter.product_mixer.core.functional_component.common.alert.predicate.TriggerIfLatencyAbove
  
  +import com.twitter.product_mixer.core.functional_component.common.alert.Alert
  
  +import com.twitter.product_mixer.core.functional_component.common.alert.LatencyAlert
  
  +import com.twitter.product_mixer.core.functional_component.common.alert.P99
  
  +import com.twitter.product_mixer.core.functional_component.common.alert.SuccessRateAlert
  
   import com.twitter.product_mixer.core.model.common.identifier.ComponentIdentifier
  
   import com.twitter.product_mixer.core.model.common.identifier.ProductPipelineIdentifier
  
   import com.twitter.product_mixer.core.model.marshalling.request
  
         requestedMaxResults = Some(params(ServerMaxResultsParam)),
  
         debugOptions = debugOptions,
  
         selectedUserIds = context.selectedUserIds,
  
  -      excludedUserIds = context.excludedUserIds
  
  +      excludedUserIds = context.excludedUserIds,
  
  +      listName = context.listName
  
       )
  
     }
  
   
  
     override def pipelineSelector(query: ListRecommendedUsersQuery): ComponentIdentifier =
  
       listRecommendedUsersMixerPipelineConfig.identifier
  
   
  
  +  override val alerts: Seq[Alert] = Seq(
  
  +    SuccessRateAlert(
  
  +      notificationGroup = DefaultNotificationGroup,
  
  +      warnPredicate = TriggerIfBelow(99.9, 20, 30),
  
  +      criticalPredicate = TriggerIfBelow(99.9, 30, 30),
  
  +    ),
  
  +    LatencyAlert(
  
  +      notificationGroup = DefaultNotificationGroup,
  
  +      percentile = P99,
  
  +      warnPredicate = TriggerIfLatencyAbove(1000.millis, 15, 30),
  
  +      criticalPredicate = TriggerIfLatencyAbove(1500.millis, 15, 30)
  
  +    )
  
  +  )
  
  +
  
     override val debugAccessPolicies: Set[AccessPolicy] = DefaultHomeMixerAccessPolicy
  
   }
  
