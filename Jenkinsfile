pipeline {
    agent any
    tools {
        jdk 'jdk17'
    }
    stages {
      stage('Code checkout from GitHub') {
        steps {
            git branch: 'jenkinsfile', url:  'https://github.com/AladdinBelhaj/achat.git'
          }
      }
      stage('Build Maven Project')  {
        steps {
           sh 'mvn clean install'     
         }
      }
   }
}
