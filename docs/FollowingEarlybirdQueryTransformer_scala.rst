a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingEarlybirdQueryTransformer.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingEarlybirdQueryTransformer.scala
==================================================

Change Range: -30,7 +30,7

Content:

::

index 9f7dd306e..c388cfa92 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingEarlybirdQueryTransformer.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/FollowingEarlybirdQueryTransformer.scala
  
   
  
   import com.twitter.finagle.thrift.ClientId
  
   import com.twitter.finagle.tracing.Trace
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.SGSFollowedUsersFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.RealGraphInNetworkScoresFeature
  
   import com.twitter.home_mixer.product.following.model.FollowingQuery
  
   import com.twitter.home_mixer.product.following.param.FollowingParam.ServerMaxResultsParam
  
  +import com.twitter.product_mixer.component_library.feature_hydrator.query.social_graph.SGSFollowedUsersFeature
  
   import com.twitter.product_mixer.core.functional_component.transformer.CandidatePipelineQueryTransformer
  
   import com.twitter.product_mixer.core.model.marshalling.response.urt.operation.BottomCursor
  
   import com.twitter.product_mixer.core.model.marshalling.response.urt.operation.GapCursor
  
       val realGraphInNetworkFollowedUserIds =
  
         query.features.map(_.get(RealGraphInNetworkScoresFeature)).getOrElse(Map.empty).keySet
  
       val userId = query.getRequiredUserId
  
  -    val combinedUserIds = userId +: (followedUserIds ++ realGraphInNetworkFollowedUserIds).toSeq
  
  +    val combinedUserIds = userId +: followedUserIds.toSeq
  
   
  
       val baseFollowedUsersSearchOperator = new SearchOperator.Builder()
  
         .setType(SearchOperator.Type.FEATURE_VALUE_IN_ACCEPT_LIST_OR_UNSET)
  
