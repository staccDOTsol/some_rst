a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/builder/ListClientEventDetailsBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/builder/ListClientEventDetailsBuilder.scala
==================================================

Change Range: -20,7 +20,7

Content:

::

index 4c7927cab..cfffee0ca 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/builder/ListClientEventDetailsBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/builder/ListClientEventDetailsBuilder.scala
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
   import com.twitter.timelineservice.suggests.{thriftscala => st}
  
   
  
  -object ListClientEventDetailsBuilder
  
  +case class ListClientEventDetailsBuilder(suggestType: st.SuggestType)
  
       extends BaseClientEventDetailsBuilder[PipelineQuery, UniversalNoun[Any]] {
  
   
  
     override def apply(
  
         conversationDetails = None,
  
         timelinesDetails = Some(
  
           TimelinesDetails(
  
  -          injectionType = Some(st.SuggestType.OrganicListTweet.name),
  
  +          injectionType = Some(suggestType.name),
  
             controllerData = None,
  
             sourceData = None)),
  
         articleDetails = None,
  
