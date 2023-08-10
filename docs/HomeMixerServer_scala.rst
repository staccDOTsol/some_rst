a/home-mixer/server/src/main/scala/com/twitter/home_mixer/HomeMixerServer.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/HomeMixerServer.scala
==================================================

Change Range: -111,6 +116,11

Content:

::

index ff7c75727..e635c7a68 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/HomeMixerServer.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/HomeMixerServer.scala
  
   import com.twitter.finatra.thrift.filters._
  
   import com.twitter.finatra.thrift.routing.ThriftRouter
  
   import com.twitter.home_mixer.controller.HomeThriftController
  
  +import com.twitter.home_mixer.federated.HomeMixerColumn
  
   import com.twitter.home_mixer.module._
  
   import com.twitter.home_mixer.param.GlobalParamConfigModule
  
   import com.twitter.home_mixer.product.HomeMixerProductModule
  
   import com.twitter.home_mixer.{thriftscala => st}
  
   import com.twitter.product_mixer.component_library.module.AccountRecommendationsMixerModule
  
  -import com.twitter.product_mixer.component_library.module.CrMixerClientModule
  
   import com.twitter.product_mixer.component_library.module.DarkTrafficFilterModule
  
   import com.twitter.product_mixer.component_library.module.EarlybirdModule
  
   import com.twitter.product_mixer.component_library.module.ExploreRankerClientModule
  
   import com.twitter.product_mixer.component_library.module.GizmoduckClientModule
  
   import com.twitter.product_mixer.component_library.module.OnboardingTaskServiceModule
  
   import com.twitter.product_mixer.component_library.module.SocialGraphServiceModule
  
  -import com.twitter.product_mixer.component_library.module.TimelineMixerClientModule
  
   import com.twitter.product_mixer.component_library.module.TimelineRankerClientModule
  
   import com.twitter.product_mixer.component_library.module.TimelineScorerClientModule
  
   import com.twitter.product_mixer.component_library.module.TimelineServiceClientModule
  
   import com.twitter.product_mixer.component_library.module.TweetImpressionStoreModule
  
  +import com.twitter.product_mixer.component_library.module.TweetMixerClientModule
  
   import com.twitter.product_mixer.component_library.module.UserSessionStoreModule
  
   import com.twitter.product_mixer.core.controllers.ProductMixerController
  
   import com.twitter.product_mixer.core.module.LoggingThrowableExceptionMapper
  
   import com.twitter.product_mixer.core.module.ProductMixerModule
  
  -import com.twitter.product_mixer.core.module.StratoClientModule
  
   import com.twitter.product_mixer.core.module.stringcenter.ProductScopeStringCenterModule
  
  +import com.twitter.strato.fed.StratoFed
  
  +import com.twitter.strato.fed.server.StratoFedServer
  
   
  
   object HomeMixerServerMain extends HomeMixerServer
  
   
  
  -class HomeMixerServer extends ThriftServer with Mtls with HttpServer with HttpMtls {
  
  +class HomeMixerServer
  
  +    extends StratoFedServer
  
  +    with ThriftServer
  
  +    with Mtls
  
  +    with HttpServer
  
  +    with HttpMtls {
  
     override val name = "home-mixer-server"
  
   
  
     override val modules: Seq[Module] = Seq(
  
       AccountRecommendationsMixerModule,
  
       AdvertiserBrandSafetySettingsStoreModule,
  
  +    BlenderClientModule,
  
       ClientSentImpressionsPublisherModule,
  
       ConversationServiceModule,
  
  -    CrMixerClientModule,
  
       EarlybirdModule,
  
       ExploreRankerClientModule,
  
  +    FeedbackHistoryClientModule,
  
       GizmoduckClientModule,
  
       GlobalParamConfigModule,
  
       HomeAdsCandidateSourceModule,
  
       HomeMixerFlagsModule,
  
       HomeMixerProductModule,
  
       HomeMixerResourcesModule,
  
  -    HomeNaviModelClientModule,
  
       ImpressionBloomFilterModule,
  
       InjectionHistoryClientModule,
  
  -    FeedbackHistoryClientModule,
  
       ManhattanClientsModule,
  
       ManhattanFeatureRepositoryModule,
  
       ManhattanTweetImpressionStoreModule,
  
       MemcachedFeatureRepositoryModule,
  
  +    NaviModelClientModule,
  
       OnboardingTaskServiceModule,
  
       OptimizedStratoClientModule,
  
       PeopleDiscoveryServiceModule,
  
       SimClustersRecentEngagementsClientModule,
  
       SocialGraphServiceModule,
  
       StaleTweetsCacheModule,
  
  -    StratoClientModule,
  
       ThriftFeatureRepositoryModule,
  
  -    TimelineMixerClientModule,
  
       TimelineRankerClientModule,
  
       TimelineScorerClientModule,
  
       TimelineServiceClientModule,
  
       TimelinesPersistenceStoreClientModule,
  
  +    TopicSocialProofClientModule,
  
       TweetImpressionStoreModule,
  
  -    TweetyPieClientModule,
  
  +    TweetMixerClientModule,
  
  +    TweetypieClientModule,
  
       TweetypieStaticEntitiesCacheClientModule,
  
  -    UserMetadataStoreModule,
  
       UserSessionStoreModule,
  
       new DarkTrafficFilterModule[st.HomeMixer.ReqRepServicePerEndpoint](),
  
       new MtlsThriftWebFormsModule[st.HomeMixer.MethodPerEndpoint](this),
  
       new ProductScopeStringCenterModule()
  
     )
  
   
  
  -  def configureThrift(router: ThriftRouter): Unit = {
  
  +  override def configureThrift(router: ThriftRouter): Unit = {
  
       router
  
         .filter[LoggingMDCFilter]
  
         .filter[TraceIdMDCFilter]
  
           this.injector,
  
           st.HomeMixer.ExecutePipeline))
  
   
  
  +  override val dest: String = "/s/home-mixer/home-mixer:strato"
  
  +
  
  +  override val columns: Seq[Class[_ <: StratoFed.Column]] =
  
  +    Seq(classOf[HomeMixerColumn])
  
  +
  
     override protected def warmup(): Unit = {
  
       handle[HomeMixerThriftServerWarmupHandler]()
  
       handle[HomeMixerHttpServerWarmupHandler]()
  
