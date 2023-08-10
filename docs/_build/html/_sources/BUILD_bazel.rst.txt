a/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/earlybird/BUILD.bazel vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/earlybird/BUILD.bazel
==================================================

Change Range: -14,5 +14,10

Content:

::

index 0ca1f89aa..2b5722179 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/earlybird/BUILD.bazel
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/earlybird/BUILD.bazel
  
           "src/thrift/com/twitter/search/common:constants-scala",
  
           "src/thrift/com/twitter/search/common:query-scala",
  
           "src/thrift/com/twitter/search/common:ranking-scala",
  
  +        "timelines/src/main/scala/com/twitter/timelines/clients/relevance_search",
  
  +        "timelines/src/main/scala/com/twitter/timelines/earlybird/common/options",
  
  +        "timelines/src/main/scala/com/twitter/timelines/earlybird/common/utils",
  
  +        "timelines/src/main/scala/com/twitter/timelines/model/types",
  
  +        "timelines/src/main/scala/com/twitter/timelines/util/stats",
  
       ],
  
   )
  
