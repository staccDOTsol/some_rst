a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/model/ScoredTweetsResponse.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/model/ScoredTweetsResponse.scala
==================================================

Change Range: -1,29 +1,6

Content:

::

index 81cdc2211..e9bd7cd61 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/model/ScoredTweetsResponse.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/model/ScoredTweetsResponse.scala
  
   package com.twitter.home_mixer.product.scored_tweets.model
  
   
  
  +import com.twitter.product_mixer.core.model.common.presentation.CandidateWithDetails
  
   import com.twitter.product_mixer.core.model.marshalling.HasMarshalling
  
  -import com.twitter.timelineservice.suggests.{thriftscala => st}
  
  -import com.twitter.tweetconvosvc.tweet_ancestor.{thriftscala => ta}
  
  -import com.twitter.timelines.render.{thriftscala => urt}
  
   
  
  -case class ScoredTweet(
  
  -  tweetId: Long,
  
  -  authorId: Long,
  
  -  score: Option[Double],
  
  -  suggestType: st.SuggestType,
  
  -  sourceTweetId: Option[Long],
  
  -  sourceUserId: Option[Long],
  
  -  quotedTweetId: Option[Long],
  
  -  quotedUserId: Option[Long],
  
  -  inReplyToTweetId: Option[Long],
  
  -  inReplyToUserId: Option[Long],
  
  -  directedAtUserId: Option[Long],
  
  -  inNetwork: Option[Boolean],
  
  -  favoritedByUserIds: Option[Seq[Long]],
  
  -  followedByUserIds: Option[Seq[Long]],
  
  -  topicId: Option[Long],
  
  -  topicFunctionalityType: Option[urt.TopicContextFunctionalityType],
  
  -  ancestors: Option[Seq[ta.TweetAncestor]],
  
  -  isReadFromCache: Option[Boolean],
  
  -  streamToKafka: Option[Boolean])
  
  -
  
  -case class ScoredTweetsResponse(scoredTweets: Seq[ScoredTweet]) extends HasMarshalling
  
  +case class ScoredTweetsResponse(scoredTweets: Seq[CandidateWithDetails]) extends HasMarshalling
  
