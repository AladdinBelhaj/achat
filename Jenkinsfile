pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'Default Maven'
    }
    
     environment {
    SONAR_TOKEN = credentials('sonar-token')
}

    stages {

        stage('Code checkout from GitHub') {
            steps {
                git branch: 'jenkinsfile',
                    url: 'https://github.com/AladdinBelhaj/achat.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh '''
                    mvn sonar:sonar -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }
    }
}
