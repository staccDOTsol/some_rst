a/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/HomeMixerFlagName.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/HomeMixerFlagName.scala
==================================================

Change Range: -2,7 +2,8

Content:

::

index afe23c35a..dc5f5513f 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/HomeMixerFlagName.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/HomeMixerFlagName.scala
  
   
  
   object HomeMixerFlagName {
  
     final val ScribeClientEventsFlag = "scribe.client_events"
  
  -  final val ScribeServedEntriesFlag = "scribe.served_entries"
  
  +  final val ScribeServedCandidatesFlag = "scribe.served_candidates"
  
  +  final val ScribeScoredCandidatesFlag = "scribe.scored_candidates"
  
     final val ScribeServedCommonFeaturesAndCandidateFeaturesFlag =
  
       "scribe.served_common_features_and_candidate_features"
  
     final val DataRecordMetadataStoreConfigsYmlFlag = "data.record.metadata.store.configs.yml"
  
