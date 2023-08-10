a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/UserLanguagesFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/UserLanguagesFeatureHydrator.scala
==================================================

Change Range: -18,17 +20,26

Content:

::

similarity index 58%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/UserLanguagesFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/UserLanguagesFeatureHydrator.scala
  
  index ad97f3a23..e2431c43e 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/UserLanguagesFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/UserLanguagesFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator
  
   
  
  -import com.twitter.home_mixer.param.HomeMixerInjectionNames.UserLanguagesStore
  
  +import com.twitter.finagle.stats.StatsReceiver
  
  +import com.twitter.home_mixer.param.HomeMixerInjectionNames.UserLanguagesRepository
  
  +import com.twitter.home_mixer.util.ObservedKeyValueResultHandler
  
   import com.twitter.product_mixer.core.feature.Feature
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMapBuilder
  
   import com.twitter.product_mixer.core.model.common.identifier.FeatureHydratorIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
   import com.twitter.search.common.constants.{thriftscala => scc}
  
  +import com.twitter.servo.repository.KeyValueRepository
  
   import com.twitter.stitch.Stitch
  
  -import com.twitter.storehaus.ReadableStore
  
   import javax.inject.Inject
  
   import javax.inject.Named
  
   import javax.inject.Singleton
  
   
  
   @Singleton
  
   case class UserLanguagesFeatureHydrator @Inject() (
  
  -  @Named(UserLanguagesStore) store: ReadableStore[Long, Seq[scc.ThriftLanguage]])
  
  -    extends QueryFeatureHydrator[PipelineQuery] {
  
  +  @Named(UserLanguagesRepository) client: KeyValueRepository[Seq[Long], Long, Seq[
  
  +    scc.ThriftLanguage
  
  +  ]],
  
  +  statsReceiver: StatsReceiver)
  
  +    extends QueryFeatureHydrator[PipelineQuery]
  
  +    with ObservedKeyValueResultHandler {
  
   
  
     override val identifier: FeatureHydratorIdentifier = FeatureHydratorIdentifier("UserLanguages")
  
   
  
     override val features: Set[Feature[_, _]] = Set(UserLanguagesFeature)
  
   
  
  +  override val statScope: String = identifier.toString
  
  +
  
     override def hydrate(query: PipelineQuery): Stitch[FeatureMap] = {
  
  -    Stitch.callFuture(store.get(query.getRequiredUserId)).map { languages =>
  
  +    val key = query.getRequiredUserId
  
  +    Stitch.callFuture(client(Seq(key))).map { result =>
  
  +      val feature =
  
  +        observedGet(key = Some(key), keyValueResult = result).map(_.getOrElse(Seq.empty))
  
         FeatureMapBuilder()
  
  -        .add(UserLanguagesFeature, languages.getOrElse(Seq.empty))
  
  +        .add(UserLanguagesFeature, feature)
  
           .build()
  
       }
  
     }
  
