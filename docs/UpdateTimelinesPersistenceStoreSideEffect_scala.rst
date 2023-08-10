a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/UpdateTimelinesPersistenceStoreSideEffect.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/UpdateTimelinesPersistenceStoreSideEffect.scala
==================================================

Change Range: -216,8 +233,11

Content:

::

index ef8a737a8..5a1bb0f6b 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/UpdateTimelinesPersistenceStoreSideEffect.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/side_effect/UpdateTimelinesPersistenceStoreSideEffect.scala
  
   import com.twitter.home_mixer.model.HomeFeatures._
  
   import com.twitter.home_mixer.model.request.FollowingProduct
  
   import com.twitter.home_mixer.model.request.ForYouProduct
  
  +import com.twitter.home_mixer.model.HomeFeatures.IsTweetPreviewFeature
  
   import com.twitter.home_mixer.service.HomeMixerAlertConfig
  
   import com.twitter.product_mixer.component_library.pipeline.candidate.who_to_follow_module.WhoToFollowCandidateDecorator
  
  +import com.twitter.product_mixer.component_library.pipeline.candidate.who_to_subscribe_module.WhoToSubscribeCandidateDecorator
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
   import com.twitter.product_mixer.core.functional_component.side_effect.PipelineResultSideEffect
  
   import com.twitter.product_mixer.core.model.common.identifier.SideEffectIdentifier
  
         val entries = inputs.response.instructions.collect {
  
           case AddEntriesTimelineInstruction(entries) =>
  
             entries.collect {
  
  -            // includes both tweets and promoted tweets
  
  -            case entry: TweetItem if entry.sortIndex.isDefined =>
  
  +            // includes tweets, tweet previews, and promoted tweets
  
  +            case entry: TweetItem if entry.sortIndex.isDefined => {
  
                 Seq(
  
                   buildTweetEntryWithItemIds(
  
                     tweetIdToItemCandidateMap(entry.id),
  
  -                  entry.sortIndex.get))
  
  +                  entry.sortIndex.get
  
  +                ))
  
  +            }
  
               // tweet conversation modules are flattened to individual tweets in the persistence store
  
               case module: TimelineModule
  
                   if module.sortIndex.isDefined && module.items.headOption.exists(
  
                     size = module.items.size.toShort,
  
                     itemIds = Some(userIds)
  
                   ))
  
  +            case module: TimelineModule
  
  +                if module.sortIndex.isDefined && module.entryNamespace.toString == WhoToSubscribeCandidateDecorator.EntryNamespaceString =>
  
  +              val userIds = module.items
  
  +                .map(item =>
  
  +                  UpdateTimelinesPersistenceStoreSideEffect.EmptyItemIds.copy(userId =
  
  +                    Some(item.item.id.asInstanceOf[Long])))
  
  +              Seq(
  
  +                EntryWithItemIds(
  
  +                  entityIdType = EntityIdType.WhoToSubscribe,
  
  +                  sortIndex = module.sortIndex.get,
  
  +                  size = module.items.size.toShort,
  
  +                  itemIds = Some(userIds)
  
  +                ))
  
             }.flatten
  
           case ShowCoverInstruction(cover) =>
  
             Seq(
  
         userId = None
  
       )
  
   
  
  +    val isPreview = features.getOrElse(IsTweetPreviewFeature, default = false)
  
  +    val entityType = if (isPreview) EntityIdType.TweetPreview else EntityIdType.Tweet
  
  +
  
       EntryWithItemIds(
  
  -      entityIdType = EntityIdType.Tweet,
  
  +      entityIdType = entityType,
  
         sortIndex = sortIndex,
  
         size = 1.toShort,
  
         itemIds = Some(Seq(itemIds))
  
