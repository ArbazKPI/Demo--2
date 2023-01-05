#! groovy

pipeline {
    agent any
    
    stages {
      stage('Setup') {
        steps {
            
          checkout scmGit(branches: [[name: '*/main']], extensions: [cloneOption(noTags: false, reference: '', shallow: false, timeout: 120)], userRemoteConfigs: [[url: 'https://github.com/ArbazKPI/Demo--2.git']])  
          
          echo "Setup"
          // Install bundler in order to use fastlane
          bat "gem install bundler"
          // set the local path for bundles in vendor/bundle
          bat "bundle config set --local path 'vendor/bundle'"
          // install bundles if they're not installed
          bat "bundle check || bundle install --jobs=4 --retry=3"
        }
      }

      stage('Build') {
        steps {
          echo "Building"
          bat "bundle exec fastlane build"
        }
      }

      stage('Deploy') {
              steps {
                echo "Deploying to Firebase"
                bat "bundle exec fastlane beta"
              }
      }
    }
}

