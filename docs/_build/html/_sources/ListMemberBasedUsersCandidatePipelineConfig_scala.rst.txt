a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/ListMemberBasedUsersCandidatePipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/ListMemberBasedUsersCandidatePipelineConfig.scala
==================================================

Change Range: -74,12 +80,15

Content:

::

index 267c0b4f3..756c58d72 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/ListMemberBasedUsersCandidatePipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/ListMemberBasedUsersCandidatePipelineConfig.scala
  
   package com.twitter.home_mixer.product.list_recommended_users
  
   
  
   import com.twitter.hermit.candidate.{thriftscala => t}
  
  -import com.twitter.home_mixer.functional_component.candidate_source.SimilarityBasedUsersCandidateSource
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.ListMembersFeature
  
  -import com.twitter.home_mixer.functional_component.filter.PredicateFeatureFilter
  
  -import com.twitter.home_mixer.product.list_recommended_users.feature_hydrator.GizmoduckUserFeatureHydrator
  
  +import com.twitter.home_mixer.product.list_recommended_users.candidate_source.SimilarityBasedUsersCandidateSource
  
  +import com.twitter.home_mixer.product.list_recommended_users.feature_hydrator.IsGizmoduckValidUserFeatureHydrator
  
   import com.twitter.home_mixer.product.list_recommended_users.feature_hydrator.IsListMemberFeatureHydrator
  
  -import com.twitter.home_mixer.product.list_recommended_users.filter.DropMaxCandidatesByScoreFilter
  
  +import com.twitter.home_mixer.product.list_recommended_users.feature_hydrator.IsSGSValidUserFeatureHydrator
  
  +import com.twitter.home_mixer.product.list_recommended_users.feature_hydrator.RecentListMembersFeature
  
  +import com.twitter.home_mixer.product.list_recommended_users.filter.DropMaxCandidatesByAggregatedScoreFilter
  
   import com.twitter.home_mixer.product.list_recommended_users.filter.PreviouslyServedUsersFilter
  
  -import com.twitter.home_mixer.product.list_recommended_users.model.ListFeatures.IsListMemberFeature
  
  +import com.twitter.home_mixer.product.list_recommended_users.model.ListRecommendedUsersFeatures.IsListMemberFeature
  
   import com.twitter.home_mixer.product.list_recommended_users.model.ListRecommendedUsersQuery
  
   import com.twitter.product_mixer.component_library.decorator.urt.UrtItemCandidateDecorator
  
   import com.twitter.product_mixer.component_library.decorator.urt.builder.item.user.UserCandidateUrtItemBuilder
  
   import com.twitter.product_mixer.component_library.decorator.urt.builder.metadata.ClientEventInfoBuilder
  
  +import com.twitter.product_mixer.component_library.filter.PredicateFeatureFilter
  
  +import com.twitter.product_mixer.component_library.gate.NonEmptySeqFeatureGate
  
   import com.twitter.product_mixer.component_library.model.candidate.UserCandidate
  
   import com.twitter.product_mixer.core.functional_component.candidate_source.BaseCandidateSource
  
   import com.twitter.product_mixer.core.functional_component.decorator.CandidateDecorator
  
   import com.twitter.product_mixer.core.functional_component.feature_hydrator.BaseCandidateFeatureHydrator
  
   import com.twitter.product_mixer.core.functional_component.filter.Filter
  
  +import com.twitter.product_mixer.core.functional_component.gate.Gate
  
   import com.twitter.product_mixer.core.functional_component.transformer.CandidateFeatureTransformer
  
   import com.twitter.product_mixer.core.functional_component.transformer.CandidatePipelineQueryTransformer
  
   import com.twitter.product_mixer.core.functional_component.transformer.CandidatePipelineResultsTransformer
  
   import com.twitter.product_mixer.core.model.common.identifier.CandidatePipelineIdentifier
  
   import com.twitter.product_mixer.core.model.common.identifier.FilterIdentifier
  
   import com.twitter.product_mixer.core.pipeline.candidate.CandidatePipelineConfig
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
   @Singleton
  
   class ListMemberBasedUsersCandidatePipelineConfig @Inject() (
  
     similarityBasedUsersCandidateSource: SimilarityBasedUsersCandidateSource,
  
  -  gizmoduckUserFeatureHydrator: GizmoduckUserFeatureHydrator,
  
  -  isListMemberFeatureHydrator: IsListMemberFeatureHydrator)
  
  +  isGizmoduckValidUserFeatureHydrator: IsGizmoduckValidUserFeatureHydrator,
  
  +  isListMemberFeatureHydrator: IsListMemberFeatureHydrator,
  
  +  isSGSValidUserFeatureHydrator: IsSGSValidUserFeatureHydrator)
  
       extends CandidatePipelineConfig[
  
         ListRecommendedUsersQuery,
  
         Seq[Long],
  
     override val identifier: CandidatePipelineIdentifier =
  
       CandidatePipelineIdentifier("ListMemberBasedUsers")
  
   
  
  +  override val gates: Seq[Gate[ListRecommendedUsersQuery]] =
  
  +    Seq(NonEmptySeqFeatureGate(RecentListMembersFeature))
  
  +
  
     override val queryTransformer: CandidatePipelineQueryTransformer[ListRecommendedUsersQuery, Seq[
  
       Long
  
     ]] = { query =>
  
  -    query.features.map(_.getOrElse(ListMembersFeature, Seq.empty)).getOrElse(Seq.empty)
  
  +    query.features.map(_.getOrElse(RecentListMembersFeature, Seq.empty)).getOrElse(Seq.empty)
  
     }
  
   
  
     override val candidateSource: BaseCandidateSource[Seq[Long], t.Candidate] =
  
           FilterIdentifier("IsListMember"),
  
           shouldKeepCandidate = { features => !features.getOrElse(IsListMemberFeature, false) }
  
         ),
  
  -      DropMaxCandidatesByScoreFilter
  
  +      DropMaxCandidatesByAggregatedScoreFilter
  
       )
  
   
  
     override val postFilterFeatureHydration: Seq[
  
       BaseCandidateFeatureHydrator[ListRecommendedUsersQuery, UserCandidate, _]
  
  -  ] = Seq(gizmoduckUserFeatureHydrator)
  
  +  ] = Seq(
  
  +    isGizmoduckValidUserFeatureHydrator,
  
  +    isSGSValidUserFeatureHydrator
  
  +  )
  
   
  
     override val decorator: Option[CandidateDecorator[ListRecommendedUsersQuery, UserCandidate]] = {
  
       val clientEventInfoBuilder = ClientEventInfoBuilder("user")
  
