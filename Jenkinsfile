pipeline {
    agent any

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
