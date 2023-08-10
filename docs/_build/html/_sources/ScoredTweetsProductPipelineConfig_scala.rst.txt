a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/ScoredTweetsProductPipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/ScoredTweetsProductPipelineConfig.scala
==================================================

Change Range: -44,19 +46,19

Content:

::

index 9870030b8..27278db0c 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/ScoredTweetsProductPipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/ScoredTweetsProductPipelineConfig.scala
  
   package com.twitter.home_mixer.product.scored_tweets
  
   
  
   import com.twitter.home_mixer.model.HomeFeatures.ServedTweetIdsFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.TimelineServiceTweetsFeature
  
   import com.twitter.home_mixer.model.request.HomeMixerRequest
  
   import com.twitter.home_mixer.model.request.ScoredTweetsProduct
  
   import com.twitter.home_mixer.model.request.ScoredTweetsProductContext
  
   import com.twitter.home_mixer.product.scored_tweets.param.ScoredTweetsParamConfig
  
   import com.twitter.home_mixer.service.HomeMixerAccessPolicy.DefaultHomeMixerAccessPolicy
  
   import com.twitter.home_mixer.{thriftscala => t}
  
  +import com.twitter.product_mixer.component_library.premarshaller.cursor.UrtCursorSerializer
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMapBuilder
  
   import com.twitter.product_mixer.core.functional_component.common.access_policy.AccessPolicy
  
   import com.twitter.product_mixer.core.model.common.identifier.ComponentIdentifier
  
         case _ => throw PipelineFailure(BadRequest, "ScoredTweetsProductContext not found")
  
       }
  
   
  
  -    val featureMap = context.servedTweetIds.map { servedTweets =>
  
  -      FeatureMapBuilder()
  
  -        .add(ServedTweetIdsFeature, servedTweets)
  
  -        .build()
  
  -    }
  
  +    val featureMap = FeatureMapBuilder()
  
  +      .add(ServedTweetIdsFeature, context.servedTweetIds.getOrElse(Seq.empty))
  
  +      .add(TimelineServiceTweetsFeature, context.backfillTweetIds.getOrElse(Seq.empty))
  
  +      .build()
  
   
  
       ScoredTweetsQuery(
  
         params = params,
  
         clientContext = request.clientContext,
  
  -      features = featureMap,
  
  -      pipelineCursor = None,
  
  +      pipelineCursor =
  
  +        request.serializedRequestCursor.flatMap(UrtCursorSerializer.deserializeOrderedCursor),
  
         requestedMaxResults = Some(params(ServerMaxResultsParam)),
  
         debugOptions = request.debugParams.flatMap(_.debugOptions),
  
  +      features = Some(featureMap),
  
         deviceContext = context.deviceContext,
  
         seenTweetIds = context.seenTweetIds,
  
         qualityFactorStatus = None
  
