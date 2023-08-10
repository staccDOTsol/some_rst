a/home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/timeline_logging/ConversationEntryMarshaller.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/timeline_logging/ConversationEntryMarshaller.scala
==================================================

Change Range: -1,16 +0,0

Content:

::

deleted file mode 100644
  
  index 8123f3597..000000000
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/timeline_logging/ConversationEntryMarshaller.scala
  
  +++ /dev/null
  
  -package com.twitter.home_mixer.marshaller.timeline_logging
  
  -
  
  -import com.twitter.home_mixer.model.HomeFeatures.ScoreFeature
  
  -import com.twitter.product_mixer.core.model.common.presentation.ItemCandidateWithDetails
  
  -import com.twitter.product_mixer.core.model.marshalling.response.urt.item.tweet.TweetItem
  
  -import com.twitter.timelines.timeline_logging.{thriftscala => thriftlog}
  
  -
  
  -object ConversationEntryMarshaller {
  
  -
  
  -  def apply(entry: TweetItem, candidate: ItemCandidateWithDetails): thriftlog.ConversationEntry =
  
  -    thriftlog.ConversationEntry(
  
  -      displayedTweetId = entry.id,
  
  -      displayType = Some(entry.displayType.toString),
  
  -      score = candidate.features.getOrElse(ScoreFeature, None)
  
  -    )
  
  -}
  
