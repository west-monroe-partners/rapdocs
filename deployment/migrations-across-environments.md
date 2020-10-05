---
description: >-
  An overview of the RAP development lifecycle and how to handle source
  migrations between the various environments.
---

# !! Environment Migration Process

!! To add: Best practices.

For RAP projects, configurators will be working with two different environments in order to keep test data and production data separate. Environment migration is the process of copying the state of one RAP environment to another so that an identically-configured environments can be created in a timely manner

### New RAP Implementations

For most RAP implementations, configurators will at first use one environment \(called DEV\) for testing, building Proof-Of-Concepts, etc. This environment will eventually become the official production environment \(PROD\) for the project. Once this happens, configurators will still need another RAP environment that mirrors the state of the production environment for testing and troubleshooting issues with the client.

The environment migration process allows configurators to complete this step much quicker than re-creating the environment from scratch. 

TODO - you have one environment that will become PROD eventually, use it as your DEV

TODO - when you need to stand up a new DEV, go through steps in Code Deployment section, then go to next section for steps to migrate code PROD -&gt; DEV

### !! Existing RAP Implementations

TODO - document export / import steps from DEV to PROD

TODO - note that import / export done on source name, why those need to be in sync \(prevent duplicates\)

