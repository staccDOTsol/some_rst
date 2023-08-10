a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/HomeMixerFlagsModule.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/HomeMixerFlagsModule.scala
==================================================

Change Range: -16,9 +16,15

Content:

::

index 4031596a7..e31d2d9bc 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/HomeMixerFlagsModule.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/HomeMixerFlagsModule.scala
  
     )
  
   
  
     flag[Boolean](
  
  -    name = ScribeServedEntriesFlag,
  
  +    name = ScribeServedCandidatesFlag,
  
       default = false,
  
  -    help = "Toggles logging served entries to Scribe"
  
  +    help = "Toggles logging served candidates to Scribe"
  
  +  )
  
  +
  
  +  flag[Boolean](
  
  +    name = ScribeScoredCandidatesFlag,
  
  +    default = false,
  
  +    help = "Toggles logging scored candidates to Scribe"
  
     )
  
   
  
     flag[Boolean](
  
