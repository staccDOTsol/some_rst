a/home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/timeline_logging/PromotedTweetEntryMarshaller.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/timeline_logging/PromotedTweetDetailsMarshaller.scala
==================================================

Change Range: -3,15 +3,13

Content:

::

similarity index 56%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/timeline_logging/PromotedTweetEntryMarshaller.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/timeline_logging/PromotedTweetDetailsMarshaller.scala
  
  index b96bb38a2..d913f8d64 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/timeline_logging/PromotedTweetEntryMarshaller.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/marshaller/timeline_logging/PromotedTweetDetailsMarshaller.scala
  
   import com.twitter.product_mixer.core.model.marshalling.response.urt.item.tweet.TweetItem
  
   import com.twitter.timelines.timeline_logging.{thriftscala => thriftlog}
  
   
  
  -object PromotedTweetEntryMarshaller {
  
  +object PromotedTweetDetailsMarshaller {
  
   
  
  -  def apply(entry: TweetItem, position: Int): thriftlog.PromotedTweetEntry = {
  
  -    thriftlog.PromotedTweetEntry(
  
  -      id = entry.id,
  
  -      advertiserId = entry.promotedMetadata.map(_.advertiserId).getOrElse(0L),
  
  -      insertPosition = position,
  
  -      impressionId = entry.promotedMetadata.flatMap(_.impressionString),
  
  -      displayType = Some(entry.displayType.toString)
  
  +  def apply(entry: TweetItem, position: Int): thriftlog.PromotedTweetDetails = {
  
  +    thriftlog.PromotedTweetDetails(
  
  +      advertiserId = Some(entry.promotedMetadata.map(_.advertiserId).getOrElse(0L)),
  
  +      insertPosition = Some(position),
  
  +      impressionId = entry.promotedMetadata.flatMap(_.impressionString)
  
       )
  
     }
  
   }
  
