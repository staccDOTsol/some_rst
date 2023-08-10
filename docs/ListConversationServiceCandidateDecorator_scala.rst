a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/ListConversationServiceCandidateDecorator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_tweets/decorator/ListConversationServiceCandidateDecorator.scala
==================================================

Change Range: -23,8 +23,10

Content:

::

similarity index 91%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/ListConversationServiceCandidateDecorator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_tweets/decorator/ListConversationServiceCandidateDecorator.scala
  
  index a8df05029..a02b628e8 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/ListConversationServiceCandidateDecorator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_tweets/decorator/ListConversationServiceCandidateDecorator.scala
  
  -package com.twitter.home_mixer.functional_component.decorator
  
  +package com.twitter.home_mixer.product.list_tweets.decorator
  
   
  
   import com.twitter.home_mixer.functional_component.decorator.builder.HomeConversationModuleMetadataBuilder
  
   import com.twitter.home_mixer.functional_component.decorator.builder.ListClientEventDetailsBuilder
  
     def apply(): Some[UrtMultipleModulesDecorator[PipelineQuery, TweetCandidate, Long]] = {
  
       val suggestType = st.SuggestType.OrganicListTweet
  
       val component = InjectionScribeUtil.scribeComponent(suggestType).get
  
  -    val clientEventInfoBuilder =
  
  -      ClientEventInfoBuilder(component, Some(ListClientEventDetailsBuilder))
  
  +    val clientEventInfoBuilder = ClientEventInfoBuilder(
  
  +      component = component,
  
  +      detailsBuilder = Some(ListClientEventDetailsBuilder(st.SuggestType.OrganicListTweet))
  
  +    )
  
       val tweetItemBuilder = TweetCandidateUrtItemBuilder(
  
         clientEventInfoBuilder = clientEventInfoBuilder
  
       )
  
