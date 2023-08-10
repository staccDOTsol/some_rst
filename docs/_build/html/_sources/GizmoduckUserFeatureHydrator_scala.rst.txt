a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/feature_hydrator/GizmoduckUserFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/feature_hydrator/IsGizmoduckValidUserFeatureHydrator.scala
==================================================

Change Range: -44,14 +44,21

Content:

::

similarity index 71%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/feature_hydrator/GizmoduckUserFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/feature_hydrator/IsGizmoduckValidUserFeatureHydrator.scala
  
  index d1db2c348..80a41adf0 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/feature_hydrator/GizmoduckUserFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/feature_hydrator/IsGizmoduckValidUserFeatureHydrator.scala
  
   package com.twitter.home_mixer.product.list_recommended_users.feature_hydrator
  
   
  
   import com.twitter.gizmoduck.{thriftscala => gt}
  
  -import com.twitter.home_mixer.product.list_recommended_users.model.ListFeatures.GizmoduckUserFeature
  
  +import com.twitter.home_mixer.product.list_recommended_users.model.ListRecommendedUsersFeatures.IsGizmoduckValidUserFeature
  
   import com.twitter.product_mixer.component_library.model.candidate.UserCandidate
  
   import com.twitter.product_mixer.core.feature.Feature
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
   import javax.inject.Singleton
  
   
  
   @Singleton
  
  -class GizmoduckUserFeatureHydrator @Inject() (gizmoduck: Gizmoduck)
  
  +class IsGizmoduckValidUserFeatureHydrator @Inject() (gizmoduck: Gizmoduck)
  
       extends BulkCandidateFeatureHydrator[PipelineQuery, UserCandidate] {
  
   
  
     override val identifier: FeatureHydratorIdentifier =
  
  -    FeatureHydratorIdentifier("GizmoduckUser")
  
  +    FeatureHydratorIdentifier("IsGizmoduckValidUser")
  
   
  
  -  override val features: Set[Feature[_, _]] = Set(GizmoduckUserFeature)
  
  +  override val features: Set[Feature[_, _]] = Set(IsGizmoduckValidUserFeature)
  
   
  
     private val queryFields: Set[gt.QueryFields] = Set(gt.QueryFields.Safety)
  
   
  
         .collectToTry(
  
           userIds.map(userId => gizmoduck.getUserById(userId, queryFields, context))).map {
  
           userResults =>
  
  -          val idToUserMap = userResults
  
  +          val idToUserSafetyMap = userResults
  
               .collect {
  
                 case Return(user) => user
  
  -            }.map(user => user.id -> user).toMap
  
  +            }.map(user => user.id -> user.safety).toMap
  
   
  
             candidates.map { candidate =>
  
  +            val safety = idToUserSafetyMap.getOrElse(candidate.candidate.id, None)
  
  +            val isValidUser = safety.isDefined &&
  
  +              !safety.exists(_.deactivated) &&
  
  +              !safety.exists(_.suspended) &&
  
  +              !safety.exists(_.isProtected) &&
  
  +              !safety.flatMap(_.offboarded).getOrElse(false)
  
  +
  
               FeatureMapBuilder()
  
  -              .add(GizmoduckUserFeature, idToUserMap.get(candidate.candidate.id))
  
  +              .add(IsGizmoduckValidUserFeature, isValidUser)
  
                 .build()
  
             }
  
         }
  
