a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/ListMembersQueryFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/feature_hydrator/RecentListMembersQueryFeatureHydrator.scala
==================================================

Change Range: -36,7 +37,7

Content:

::

similarity index 72%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/ListMembersQueryFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/feature_hydrator/RecentListMembersQueryFeatureHydrator.scala
  
  index a6d3324d3..81a5ce504 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/ListMembersQueryFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/feature_hydrator/RecentListMembersQueryFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.list_recommended_users.feature_hydrator
  
   
  
   import com.twitter.home_mixer.model.request.HasListId
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
  -case object ListMembersFeature extends FeatureWithDefaultOnFailure[PipelineQuery, Seq[Long]] {
  
  +case object RecentListMembersFeature extends FeatureWithDefaultOnFailure[PipelineQuery, Seq[Long]] {
  
     override val defaultValue: Seq[Long] = Seq.empty
  
   }
  
   
  
   @Singleton
  
  -class ListMembersQueryFeatureHydrator @Inject() (socialGraph: SocialGraph)
  
  +class RecentListMembersQueryFeatureHydrator @Inject() (socialGraph: SocialGraph)
  
       extends QueryFeatureHydrator[PipelineQuery with HasListId] {
  
   
  
  -  override val identifier: FeatureHydratorIdentifier = FeatureHydratorIdentifier("ListMembers")
  
  +  override val identifier: FeatureHydratorIdentifier =
  
  +    FeatureHydratorIdentifier("RecentListMembers")
  
   
  
  -  override val features: Set[Feature[_, _]] = Set(ListMembersFeature)
  
  +  override val features: Set[Feature[_, _]] = Set(RecentListMembersFeature)
  
   
  
     private val MaxRecentMembers = 10
  
   
  
         pageRequest = Some(sg.PageRequest(selectAll = Some(true), count = Some(MaxRecentMembers)))
  
       )
  
       socialGraph.ids(request).map(_.ids).map { listMembers =>
  
  -      FeatureMapBuilder().add(ListMembersFeature, listMembers).build()
  
  +      FeatureMapBuilder().add(RecentListMembersFeature, listMembers).build()
  
       }
  
     }
  
   }
  
