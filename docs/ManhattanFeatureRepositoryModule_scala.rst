a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ManhattanFeatureRepositoryModule.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ManhattanFeatureRepositoryModule.scala
==================================================

Change Range: -430,24 +451,18

Content:

::

index f9afc1dcb..5668ba0ee 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ManhattanFeatureRepositoryModule.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ManhattanFeatureRepositoryModule.scala
  
   import com.twitter.finagle.mtls.authentication.ServiceIdentifier
  
   import com.twitter.home_mixer.param.HomeMixerInjectionNames._
  
   import com.twitter.home_mixer.util.InjectionTransformerImplicits._
  
  +import com.twitter.home_mixer.util.LanguageUtil
  
   import com.twitter.home_mixer.util.TensorFlowUtil
  
   import com.twitter.inject.TwitterModule
  
  -import com.twitter.manhattan.v1.thriftscala.ManhattanCoordinator
  
   import com.twitter.manhattan.v1.{thriftscala => mh}
  
  -import com.twitter.ml.api.thriftscala.FloatTensor
  
   import com.twitter.ml.api.{thriftscala => ml}
  
   import com.twitter.ml.featurestore.lib.UserId
  
  -import com.twitter.ml.featurestore.thriftscala.EntityId
  
  +import com.twitter.ml.featurestore.{thriftscala => fs}
  
   import com.twitter.onboarding.relevance.features.{thriftjava => rf}
  
   import com.twitter.product_mixer.shared_library.manhattan_client.ManhattanClientBuilder
  
   import com.twitter.scalding_internal.multiformat.format.keyval.KeyValInjection.ScalaBinaryThrift
  
  +import com.twitter.search.common.constants.{thriftscala => scc}
  
  +import com.twitter.service.metastore.gen.{thriftscala => smg}
  
   import com.twitter.servo.cache._
  
   import com.twitter.servo.manhattan.ManhattanKeyValueRepository
  
   import com.twitter.servo.repository.CachingKeyValueRepository
  
   import com.twitter.servo.repository.Repository
  
   import com.twitter.servo.repository.keysAsQuery
  
   import com.twitter.servo.util.Transformer
  
  +import com.twitter.storage.client.manhattan.bijections.Bijections
  
   import com.twitter.storehaus_internal.manhattan.ManhattanClusters
  
   import com.twitter.timelines.author_features.v1.{thriftjava => af}
  
  -import com.twitter.timelines.suggests.common.dense_data_record.thriftscala.DenseFeatureMetadata
  
  -import com.twitter.user_session_store.thriftscala.UserSession
  
  +import com.twitter.timelines.suggests.common.dense_data_record.{thriftscala => ddr}
  
  +import com.twitter.user_session_store.{thriftscala => uss_scala}
  
   import com.twitter.user_session_store.{thriftjava => uss}
  
   import com.twitter.util.Duration
  
   import com.twitter.util.Try
  
   
  
     private val DEFAULT_RPC_CHUNK_SIZE = 50
  
   
  
  -  private val ThriftEntityIdInjection = ScalaBinaryThrift(EntityId)
  
  +  private val ThriftEntityIdInjection = ScalaBinaryThrift(fs.EntityId)
  
   
  
  -  val UserIdKeyTransformer = new Transformer[Long, ByteBuffer] {
  
  +  private val FeatureStoreUserIdKeyTransformer = new Transformer[Long, ByteBuffer] {
  
       override def to(userId: Long): Try[ByteBuffer] = {
  
         Try(ByteBuffer.wrap(ThriftEntityIdInjection.apply(UserId(userId).toThrift)))
  
       }
  
       override def from(b: ByteBuffer): Try[Long] = ???
  
     }
  
   
  
  -  val FloatTensorTransformer = new Transformer[ByteBuffer, FloatTensor] {
  
  -    override def to(input: ByteBuffer): Try[FloatTensor] = {
  
  +  private val FloatTensorTransformer = new Transformer[ByteBuffer, ml.FloatTensor] {
  
  +    override def to(input: ByteBuffer): Try[ml.FloatTensor] = {
  
         val floatTensor = TensorFlowUtil.embeddingByteBufferToFloatTensor(input)
  
         Try(floatTensor)
  
       }
  
   
  
  -    override def from(b: FloatTensor): Try[ByteBuffer] = ???
  
  +    override def from(b: ml.FloatTensor): Try[ByteBuffer] = ???
  
     }
  
   
  
  +  private val LanguageTransformer = new Transformer[ByteBuffer, Seq[scc.ThriftLanguage]] {
  
  +    override def to(input: ByteBuffer): Try[Seq[scc.ThriftLanguage]] = {
  
  +      Try.fromScala(
  
  +        Bijections
  
  +          .BinaryScalaInjection(smg.UserLanguages)
  
  +          .andThen(Bijections.byteBuffer2Buf.inverse)
  
  +          .invert(input).map(LanguageUtil.computeLanguages(_)))
  
  +    }
  
  +
  
  +    override def from(b: Seq[scc.ThriftLanguage]): Try[ByteBuffer] = ???
  
  +  }
  
  +
  
  +  private val LongKeyTransformer = Injection
  
  +    .connect[Long, Array[Byte]]
  
  +    .toByteBufferTransformer()
  
  +
  
     // manhattan clients
  
   
  
     @Provides
  
       @Named(ManhattanStarbuckClient) client: mh.ManhattanCoordinator.MethodPerEndpoint
  
     ): KeyValueRepository[Seq[Long], Long, rf.MCUserCountingFeatures] = {
  
   
  
  -    val keyTransformer = Injection
  
  -      .connect[Long, Array[Byte]]
  
  -      .toByteBufferTransformer()
  
  -
  
       val valueTransformer = ThriftCodec
  
  -      .toCompact[rf.MCUserCountingFeatures]
  
  +      .toBinary[rf.MCUserCountingFeatures]
  
         .toByteBufferTransformer()
  
         .flip
  
   
  
       batchedManhattanKeyValueRepository[Long, rf.MCUserCountingFeatures](
  
         client = client,
  
  -      keyTransformer = keyTransformer,
  
  +      keyTransformer = LongKeyTransformer,
  
         valueTransformer = valueTransformer,
  
         appId = "wtf_ml",
  
         dataset = "mc_user_counting_features_v0_starbuck",
  
     @Named(TimelineAggregateMetadataRepository)
  
     def providesTimelineAggregateMetadataRepository(
  
       @Named(ManhattanAthenaClient) client: mh.ManhattanCoordinator.MethodPerEndpoint
  
  -  ): Repository[Int, Option[DenseFeatureMetadata]] = {
  
  +  ): Repository[Int, Option[ddr.DenseFeatureMetadata]] = {
  
   
  
       val keyTransformer = Injection
  
         .connect[Int, Array[Byte]]
  
         .toByteBufferTransformer()
  
   
  
  -    val valueTransformer = new Transformer[ByteBuffer, DenseFeatureMetadata] {
  
  +    val valueTransformer = new Transformer[ByteBuffer, ddr.DenseFeatureMetadata] {
  
         private val compactProtocolFactory = new TCompactProtocol.Factory
  
   
  
  -      def to(buffer: ByteBuffer): Try[DenseFeatureMetadata] = Try {
  
  +      def to(buffer: ByteBuffer): Try[ddr.DenseFeatureMetadata] = Try {
  
           val transport = transportFromByteBuffer(buffer)
  
  -        DenseFeatureMetadata.decode(compactProtocolFactory.getProtocol(transport))
  
  +        ddr.DenseFeatureMetadata.decode(compactProtocolFactory.getProtocol(transport))
  
         }
  
   
  
         // Encoding intentionally not implemented as it is never used
  
  -      def from(metadata: DenseFeatureMetadata): Try[ByteBuffer] = ???
  
  +      def from(metadata: ddr.DenseFeatureMetadata): Try[ByteBuffer] = ???
  
       }
  
   
  
  -    val inProcessCache: Cache[Int, Cached[DenseFeatureMetadata]] = InProcessLruCacheFactory(
  
  +    val inProcessCache: Cache[Int, Cached[ddr.DenseFeatureMetadata]] = InProcessLruCacheFactory(
  
         ttl = Duration.fromMinutes(20),
  
         lruSize = 30
  
       ).apply(serializer = Transformer(_ => ???, _ => ???)) // Serialization is not necessary here.
  
   
  
       KeyValueRepository
  
         .singular(
  
  -        new CachingKeyValueRepository[Seq[Int], Int, DenseFeatureMetadata](
  
  +        new CachingKeyValueRepository[Seq[Int], Int, ddr.DenseFeatureMetadata](
  
             keyValueRepository,
  
             new NonLockingCache(inProcessCache),
  
             keysAsQuery[Int]
  
     @Singleton
  
     @Named(RealGraphFeatureRepository)
  
     def providesRealGraphFeatureRepository(
  
  -    @Named(ManhattanApolloClient) client: mh.ManhattanCoordinator.MethodPerEndpoint
  
  -  ): Repository[Long, Option[UserSession]] = {
  
  -    val valueTransformer = CompactScalaCodec(UserSession).toByteBufferTransformer().flip
  
  +    @Named(ManhattanAthenaClient) client: mh.ManhattanCoordinator.MethodPerEndpoint
  
  +  ): Repository[Long, Option[uss_scala.UserSession]] = {
  
  +    val valueTransformer = CompactScalaCodec(uss_scala.UserSession).toByteBufferTransformer().flip
  
   
  
       KeyValueRepository.singular(
  
         new ManhattanKeyValueRepository(
  
           client = client,
  
  -        keyTransformer = UserIdKeyTransformer,
  
  +        keyTransformer = LongKeyTransformer,
  
           valueTransformer = valueTransformer,
  
           appId = "real_graph",
  
  -        dataset = "real_graph_user_features",
  
  +        dataset = "split_real_graph_features",
  
           timeoutInMillis = 100,
  
         )
  
       )
  
       @Named(HomeAuthorFeaturesCacheClient) cacheClient: Memcache
  
     ): KeyValueRepository[Seq[Long], Long, af.AuthorFeatures] = {
  
   
  
  -    val keyTransformer = Injection
  
  -      .connect[Long, Array[Byte]]
  
  -      .toByteBufferTransformer()
  
  -
  
       val valueInjection = ThriftCodec
  
         .toCompact[af.AuthorFeatures]
  
   
  
       val keyValueRepository = batchedManhattanKeyValueRepository(
  
         client = client,
  
  -      keyTransformer = keyTransformer,
  
  +      keyTransformer = LongKeyTransformer,
  
         valueTransformer = valueInjection.toByteBufferTransformer().flip,
  
         appId = "timelines_author_feature_store_athena",
  
         dataset = "timelines_author_features",
  
   
  
     @Provides
  
     @Singleton
  
  -  @Named(TwhinAuthorFollow20200101FeatureRepository)
  
  -  def providesTwhinAuthorFollow20200101FeatureRepository(
  
  +  @Named(TwhinAuthorFollowFeatureRepository)
  
  +  def providesTwhinAuthorFollowFeatureRepository(
  
       @Named(ManhattanApolloClient) client: mh.ManhattanCoordinator.MethodPerEndpoint,
  
  -    @Named(TwhinAuthorFollow20200101FeatureCacheClient) cacheClient: Memcache
  
  -  ): KeyValueRepository[Seq[Long], Long, ml.Embedding] = {
  
  -
  
  -    val keyTransformer = Injection
  
  -      .connect[Long, Array[Byte]]
  
  -      .toByteBufferTransformer()
  
  +    @Named(TwhinAuthorFollowFeatureCacheClient) cacheClient: Memcache
  
  +  ): KeyValueRepository[Seq[Long], Long, ml.FloatTensor] = {
  
  +    val keyValueRepository =
  
  +      batchedManhattanKeyValueRepository(
  
  +        client = client,
  
  +        keyTransformer = FeatureStoreUserIdKeyTransformer,
  
  +        valueTransformer = FloatTensorTransformer,
  
  +        appId = "ml_features_apollo",
  
  +        dataset = "twhin_author_follow_embedding_fsv1__v1_thrift__embedding",
  
  +        timeoutInMillis = 100
  
  +      )
  
   
  
  -    val valueInjection: Injection[ml.Embedding, Array[Byte]] =
  
  -      BinaryScalaCodec(ml.Embedding)
  
  -
  
  -    val keyValueRepository = batchedManhattanKeyValueRepository(
  
  -      client = client,
  
  -      keyTransformer = keyTransformer,
  
  -      valueTransformer = valueInjection.toByteBufferTransformer().flip,
  
  -      appId = "twhin",
  
  -      dataset = "twhinauthor_follow_0101",
  
  -      timeoutInMillis = 100
  
  -    )
  
  +    val valueInjection: Injection[ml.FloatTensor, Array[Byte]] =
  
  +      BinaryScalaCodec(ml.FloatTensor)
  
   
  
       buildMemCachedRepository(
  
         keyValueRepository = keyValueRepository,
  
         cacheClient = cacheClient,
  
  -      cachePrefix = "TwhinAuthorFollow20200101FeatureHydrator",
  
  -      ttl = 48.hours,
  
  +      cachePrefix = "twhinAuthorFollows",
  
  +      ttl = 24.hours,
  
         valueInjection = valueInjection
  
       )
  
     }
  
   
  
  +  @Provides
  
  +  @Singleton
  
  +  @Named(UserLanguagesRepository)
  
  +  def providesUserLanguagesFeatureRepository(
  
  +    @Named(ManhattanStarbuckClient) client: mh.ManhattanCoordinator.MethodPerEndpoint
  
  +  ): KeyValueRepository[Seq[Long], Long, Seq[scc.ThriftLanguage]] = {
  
  +    batchedManhattanKeyValueRepository(
  
  +      client = client,
  
  +      keyTransformer = LongKeyTransformer,
  
  +      valueTransformer = LanguageTransformer,
  
  +      appId = "user_metadata",
  
  +      dataset = "languages",
  
  +      timeoutInMillis = 70
  
  +    )
  
  +  }
  
  +
  
     @Provides
  
     @Singleton
  
     @Named(TwhinUserFollowFeatureRepository)
  
     def providesTwhinUserFollowFeatureRepository(
  
       @Named(ManhattanApolloClient) client: mh.ManhattanCoordinator.MethodPerEndpoint
  
  -  ): KeyValueRepository[Seq[Long], Long, FloatTensor] = {
  
  -
  
  +  ): KeyValueRepository[Seq[Long], Long, ml.FloatTensor] = {
  
       batchedManhattanKeyValueRepository(
  
         client = client,
  
  -      keyTransformer = UserIdKeyTransformer,
  
  +      keyTransformer = FeatureStoreUserIdKeyTransformer,
  
         valueTransformer = FloatTensorTransformer,
  
         appId = "ml_features_apollo",
  
         dataset = "twhin_user_follow_embedding_fsv1__v1_thrift__embedding",
  
     @Named(TwhinUserEngagementFeatureRepository)
  
     def providesTwhinUserEngagementFeatureRepository(
  
       @Named(ManhattanApolloClient) client: mh.ManhattanCoordinator.MethodPerEndpoint
  
  -  ): KeyValueRepository[Seq[Long], Long, FloatTensor] = {
  
  +  ): KeyValueRepository[Seq[Long], Long, ml.FloatTensor] = {
  
   
  
       batchedManhattanKeyValueRepository(
  
         client = client,
  
  -      keyTransformer = UserIdKeyTransformer,
  
  +      keyTransformer = FeatureStoreUserIdKeyTransformer,
  
         valueTransformer = FloatTensorTransformer,
  
         appId = "ml_features_apollo",
  
         dataset = "twhin_user_engagement_embedding_fsv1__v1_thrift__embedding",
  
     }
  
   
  
     private def batchedManhattanKeyValueRepository[K, V](
  
  -    client: ManhattanCoordinator.MethodPerEndpoint,
  
  +    client: mh.ManhattanCoordinator.MethodPerEndpoint,
  
       keyTransformer: Transformer[K, ByteBuffer],
  
       valueTransformer: Transformer[ByteBuffer, V],
  
       appId: String,
  
       mhDataset: String,
  
       mhAppId: String
  
     ): Repository[Long, Option[uss.UserSession]] = {
  
  -    val keyTransformer = Injection
  
  -      .connect[Long, Array[Byte]]
  
  -      .toByteBufferTransformer()
  
  -
  
       val valueInjection = ThriftCodec
  
         .toCompact[uss.UserSession]
  
   
  
       KeyValueRepository.singular(
  
         new ManhattanKeyValueRepository(
  
           client = mhClient,
  
  -        keyTransformer = keyTransformer,
  
  +        keyTransformer = LongKeyTransformer,
  
           valueTransformer = valueInjection.toByteBufferTransformer().flip,
  
           appId = mhAppId,
  
           dataset = mhDataset,
  
           timeoutInMillis = 100
  
         )
  
       )
  
  -
  
     }
  
  -
  
   }
  
