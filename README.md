# This repo will be used as a sample project for Azure DevOps 

Post-deployment inline script in the Release pipeline
cp -rf /home/site/wwwroot/package/* /home/site/wwwroot/

Note: You must set the app settings as below to disable all file caching:

WEBSITE_DYNAMIC_CACHE=0
WEBSITE_LOCAL_CACHE_OPTION=Never

Build Pipeline YAML code:
trigger: 
- main

stages:
- stage: Build
  jobs:
  - job: Build
    pool:
      vmImage: 'ubuntu-latest'
    steps:
    - task: Npm@1
      inputs:
        command: 'custom'
        customCommand: 'install -D tailwindcss postcss autoprefixer'
    - task: Npm@1
      inputs:
        command: 'custom'
        customCommand: 'run build'

    - task: Npm@1
      inputs:
        command: 'publish'
        workingDir: './dist'
        publishRegistry: 'useFeed'
        publishFeed: '$FEED_DETAILS'
