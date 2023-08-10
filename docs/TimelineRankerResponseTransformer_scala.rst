a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/response_transformer/TimelineRankerResponseTransformer.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/response_transformer/TimelineRankerResponseTransformer.scala
==================================================

Change Range: -79,7 +80,6

Content:

::

index e431ef6f4..a261b2fc2 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/response_transformer/TimelineRankerResponseTransformer.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/response_transformer/TimelineRankerResponseTransformer.scala
  
   import com.twitter.home_mixer.model.HomeFeatures.DirectedAtUserIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.EarlybirdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.EarlybirdScoreFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.ExclusiveConversationAuthorIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.FromInNetworkSourceFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.HasImageFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.HasVideoFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.MentionUserIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.QuotedTweetIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.QuotedUserIdFeature
  
  -import com.twitter.home_mixer.model.HomeFeatures.SemanticAnnotationFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.SourceTweetIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.SourceUserIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.StreamToKafkaFeature
  
       DirectedAtUserIdFeature,
  
       EarlybirdFeature,
  
       EarlybirdScoreFeature,
  
  +    ExclusiveConversationAuthorIdFeature,
  
       FromInNetworkSourceFeature,
  
       HasImageFeature,
  
       HasVideoFeature,
  
       IsRetweetFeature,
  
       MentionScreenNameFeature,
  
       MentionUserIdFeature,
  
  -    SemanticAnnotationFeature,
  
       StreamToKafkaFeature,
  
       QuotedTweetIdFeature,
  
       QuotedUserIdFeature,
  
   
  
     def transform(candidate: tlr.CandidateTweet): FeatureMap = {
  
       val tweet = candidate.tweet
  
  -    val quotedTweet = tweet.flatMap(_.quotedTweet)
  
  +    val quotedTweet = tweet.filter(_.quotedTweet.exists(_.tweetId != 0)).flatMap(_.quotedTweet)
  
       val mentions = tweet.flatMap(_.mentions).getOrElse(Seq.empty)
  
       val coreData = tweet.flatMap(_.coreData)
  
       val share = coreData.flatMap(_.share)
  
       val reply = coreData.flatMap(_.reply)
  
  -    val semanticAnnotations =
  
  -      tweet.flatMap(_.escherbirdEntityAnnotations.map(_.entityAnnotations)).getOrElse(Seq.empty)
  
   
  
       FeatureMapBuilder()
  
         .add(AuthorIdFeature, coreData.map(_.userId))
  
         .add(DirectedAtUserIdFeature, coreData.flatMap(_.directedAtUser.map(_.userId)))
  
         .add(EarlybirdFeature, candidate.features)
  
         .add(EarlybirdScoreFeature, candidate.features.map(_.earlybirdScore))
  
  +      .add(
  
  +        ExclusiveConversationAuthorIdFeature,
  
  +        tweet.flatMap(_.exclusiveTweetControl.map(_.conversationAuthorId)))
  
         .add(FromInNetworkSourceFeature, false)
  
         .add(HasImageFeature, tweet.exists(TweetMediaFeaturesExtractor.hasImage))
  
         .add(HasVideoFeature, tweet.exists(TweetMediaFeaturesExtractor.hasVideo))
  
         .add(IsRetweetFeature, share.isDefined)
  
         .add(MentionScreenNameFeature, mentions.map(_.screenName))
  
         .add(MentionUserIdFeature, mentions.flatMap(_.userId))
  
  -      .add(SemanticAnnotationFeature, semanticAnnotations)
  
         .add(StreamToKafkaFeature, true)
  
         .add(QuotedTweetIdFeature, quotedTweet.map(_.tweetId))
  
         .add(QuotedUserIdFeature, quotedTweet.map(_.userId))
  
