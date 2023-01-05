#! groovy

pipeline {
    agent any
    
    stages {
      stage('Setup') {
        steps {
            
          checkout scmGit(branches: [[name: '*/main']], extensions: [cloneOption(noTags: false, reference: '', shallow: false, timeout: 120)], userRemoteConfigs: [[url: 'https://github.com/ArbazKPI/Demo--2.git']])  
          
          echo "Setup"
          // Install bundler in order to use fastlane
          sh "gem install bundler"
          // set the local path for bundles in vendor/bundle
          sh "bundle config set --local path 'vendor/bundle'"
          // install bundles if they're not installed
          sh "bundle check || bundle install --jobs=4 --retry=3"
        }
      }

      stage('Build') {
        steps {
          echo "Building"
          sh "bundle exec fastlane build"
        }
      }

      stage('Deploy') {
              steps {
                echo "Deploying to Firebase"
                sh "bundle exec fastlane beta"
              }
      }
    }
}

