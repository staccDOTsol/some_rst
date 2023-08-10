a/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/DataRecordUtil.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/DataRecordUtil.scala
==================================================

Change Range: -0,0 +1,31

Content:

::

new file mode 100644
  
  index 000000000..b972b8158
  
  --- /dev/null
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/DataRecordUtil.scala
  
  +package com.twitter.home_mixer.util
  
  +
  
  +import com.twitter.ml.api.DataRecord
  
  +import com.twitter.ml.api.FeatureContext
  
  +import com.twitter.ml.api.util.SRichDataRecord
  
  +import com.twitter.ml.api.Feature
  
  +import java.lang.{Double => JDouble}
  
  +
  
  +object DataRecordUtil {
  
  +  def applyRename(
  
  +    dataRecord: DataRecord,
  
  +    featureContext: FeatureContext,
  
  +    renamedFeatureContext: FeatureContext,
  
  +    featureRenamingMap: Map[Feature[_], Feature[_]]
  
  +  ): DataRecord = {
  
  +    val richFullDr = new SRichDataRecord(dataRecord, featureContext)
  
  +    val richNewDr = new SRichDataRecord(new DataRecord, renamedFeatureContext)
  
  +    val featureIterator = featureContext.iterator()
  
  +    featureIterator.forEachRemaining { feature =>
  
  +      if (richFullDr.hasFeature(feature)) {
  
  +        val renamedFeature = featureRenamingMap.getOrElse(feature, feature)
  
  +
  
  +        val typedFeature = feature.asInstanceOf[Feature[JDouble]]
  
  +        val typedRenamedFeature = renamedFeature.asInstanceOf[Feature[JDouble]]
  
  +
  
  +        richNewDr.setFeatureValue(typedRenamedFeature, richFullDr.getFeatureValue(typedFeature))
  
  +      }
  
  +    }
  
  +    richNewDr.getRecord
  
  +  }
  
  +}
  
