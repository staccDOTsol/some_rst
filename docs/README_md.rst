a/home-mixer/README.md vs b/home-mixer/README.md
==================================================

Change Range: -99,4 +99,3

Content:

::

             - ScoredTweetsRecommendationPipelineConfig (main Tweet recommendation layer)
  
                   - Fetch Tweet Candidates
  
                       - ScoredTweetsInNetworkCandidatePipelineConfig
  
  -                    - ScoredTweetsCrMixerCandidatePipelineConfig
  
  +                    - ScoredTweetsTweetMixerCandidatePipelineConfig
  
                       - ScoredTweetsUtegCandidatePipelineConfig
  
                       - ScoredTweetsFrsCandidatePipelineConfig
  
                   - Feature Hydration and Scoring
  
           - ListTweetsTimelineServiceCandidatePipelineConfig (fetch tweets from timeline service)
  
           - ConversationServiceCandidatePipelineConfig (fetch ancestors for conversation modules)
  
           - ListTweetsAdsCandidatePipelineConfig (fetch ads)
  
  -
  
