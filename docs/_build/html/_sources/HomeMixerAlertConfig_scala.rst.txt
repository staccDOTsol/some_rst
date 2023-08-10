a/home-mixer/server/src/main/scala/com/twitter/home_mixer/service/HomeMixerAlertConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/service/HomeMixerAlertConfig.scala
==================================================

Change Range: -26,7 +26,8

Content:

::

index 93c361516..597fd4d36 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/service/HomeMixerAlertConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/service/HomeMixerAlertConfig.scala
  
     object BusinessHours {
  
       val DefaultNotificationGroup: NotificationGroup = NotificationGroup(
  
         warn = Destination(emails = Seq("")),
  
  -      critical = Destination(emails = Seq(""))
  
  +      critical = Destination(emails =
  
  +        Seq(""))
  
       )
  
   
  
       def defaultEmptyResponseRateAlert(warnThreshold: Double = 50, criticalThreshold: Double = 80) =
  
