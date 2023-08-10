a/home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/timeline_logging/WhoToFollowEntryMarshaller.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/timeline_logging/WhoToFollowDetailsMarshaller.scala
==================================================

Change Range: -1,17 +1,13

Content:

::

similarity index 62%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/timeline_logging/WhoToFollowEntryMarshaller.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/timeline_logging/WhoToFollowDetailsMarshaller.scala
  
  index 9a253b726..4c0b4dd1b 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/timeline_logging/WhoToFollowEntryMarshaller.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/timeline_logging/WhoToFollowDetailsMarshaller.scala
  
   package com.twitter.home_mixer.marshaller.timeline_logging
  
   
  
  -import com.twitter.product_mixer.component_library.pipeline.candidate.who_to_follow_module.ScoreFeature
  
   import com.twitter.product_mixer.core.model.common.presentation.ItemCandidateWithDetails
  
   import com.twitter.product_mixer.core.model.marshalling.response.urt.item.user.UserItem
  
   import com.twitter.timelines.timeline_logging.{thriftscala => thriftlog}
  
   
  
  -object WhoToFollowEntryMarshaller {
  
  +object WhoToFollowDetailsMarshaller {
  
   
  
  -  def apply(entry: UserItem, candidate: ItemCandidateWithDetails): thriftlog.WhoToFollowEntry =
  
  -    thriftlog.WhoToFollowEntry(
  
  -      userId = entry.id,
  
  -      displayType = Some(entry.displayType.toString),
  
  -      score = candidate.features.getOrElse(ScoreFeature, None),
  
  +  def apply(entry: UserItem, candidate: ItemCandidateWithDetails): thriftlog.WhoToFollowDetails =
  
  +    thriftlog.WhoToFollowDetails(
  
         enableReactiveBlending = entry.enableReactiveBlending,
  
         impressionId = entry.promotedMetadata.flatMap(_.impressionString),
  
         advertiserId = entry.promotedMetadata.map(_.advertiserId)
  
