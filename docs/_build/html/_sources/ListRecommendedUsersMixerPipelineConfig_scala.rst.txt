a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/ListRecommendedUsersMixerPipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/ListRecommendedUsersMixerPipelineConfig.scala
==================================================

Change Range: -46,22 +48,31

Content:

::

index d370c1057..8c72aefec 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/ListRecommendedUsersMixerPipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/ListRecommendedUsersMixerPipelineConfig.scala
  
   package com.twitter.home_mixer.product.list_recommended_users
  
   
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.ListMembersQueryFeatureHydrator
  
  -import com.twitter.home_mixer.functional_component.gate.ViewerIsListOwnerGate
  
  -import com.twitter.home_mixer.product.list_recommended_users.model.ListFeatures.GizmoduckUserFeature
  
  +import com.twitter.home_mixer.product.list_recommended_users.feature_hydrator.RecentListMembersQueryFeatureHydrator
  
  +import com.twitter.home_mixer.product.list_recommended_users.gate.ViewerIsListOwnerGate
  
  +import com.twitter.home_mixer.product.list_recommended_users.model.ListRecommendedUsersFeatures.IsGizmoduckValidUserFeature
  
  +import com.twitter.home_mixer.product.list_recommended_users.model.ListRecommendedUsersFeatures.IsSGSValidUserFeature
  
   import com.twitter.home_mixer.product.list_recommended_users.model.ListRecommendedUsersQuery
  
   import com.twitter.home_mixer.product.list_recommended_users.param.ListRecommendedUsersParam.ExcludedIdsMaxLengthParam
  
   import com.twitter.home_mixer.product.list_recommended_users.param.ListRecommendedUsersParam.ServerMaxResultsParam
  
   @Singleton
  
   class ListRecommendedUsersMixerPipelineConfig @Inject() (
  
     listMemberBasedUsersCandidatePipelineConfig: ListMemberBasedUsersCandidatePipelineConfig,
  
  +  blenderUsersCandidatePipelineConfig: BlenderUsersCandidatePipelineConfig,
  
     viewerIsListOwnerGate: ViewerIsListOwnerGate,
  
  -  listMembersQueryFeatureHydrator: ListMembersQueryFeatureHydrator,
  
  +  recentListMembersQueryFeatureHydrator: RecentListMembersQueryFeatureHydrator,
  
     urtTransportMarshaller: UrtTransportMarshaller)
  
       extends MixerPipelineConfig[ListRecommendedUsersQuery, Timeline, urt.TimelineResponse] {
  
   
  
     override val gates = Seq(viewerIsListOwnerGate)
  
   
  
     override val fetchQueryFeatures: Seq[QueryFeatureHydrator[ListRecommendedUsersQuery]] =
  
  -    Seq(listMembersQueryFeatureHydrator)
  
  +    Seq(recentListMembersQueryFeatureHydrator)
  
   
  
     override val candidatePipelines: Seq[
  
       CandidatePipelineConfig[ListRecommendedUsersQuery, _, _, _]
  
  -  ] =
  
  -    Seq(listMemberBasedUsersCandidatePipelineConfig)
  
  +  ] = Seq(
  
  +    listMemberBasedUsersCandidatePipelineConfig,
  
  +    blenderUsersCandidatePipelineConfig
  
  +  )
  
  +
  
  +  private val candidatePipelineIdentifiers = Set(
  
  +    listMemberBasedUsersCandidatePipelineConfig.identifier,
  
  +    blenderUsersCandidatePipelineConfig.identifier
  
  +  )
  
   
  
     override val resultSelectors: Seq[Selector[ListRecommendedUsersQuery]] = Seq(
  
       DropFilteredCandidates(
  
  -      candidatePipeline = listMemberBasedUsersCandidatePipelineConfig.identifier,
  
  -      filter = candidate => candidate.features.getOrElse(GizmoduckUserFeature, None).isDefined
  
  +      candidatePipelines = candidatePipelineIdentifiers,
  
  +      filter = candidate =>
  
  +        candidate.features.getOrElse(IsSGSValidUserFeature, false) &&
  
  +          candidate.features.getOrElse(IsGizmoduckValidUserFeature, false)
  
       ),
  
       DropMaxCandidates(
  
  -      candidatePipeline = listMemberBasedUsersCandidatePipelineConfig.identifier,
  
  +      candidatePipelines = candidatePipelineIdentifiers,
  
         maxSelectionsParam = ServerMaxResultsParam),
  
  -    InsertAppendResults(listMemberBasedUsersCandidatePipelineConfig.identifier)
  
  +    InsertAppendResults(candidatePipelineIdentifiers)
  
     )
  
   
  
     override val domainMarshaller: DomainMarshaller[ListRecommendedUsersQuery, Timeline] = {
  
