pipeline {
    agent any

    tools {
        jdk 'jdk17'
    }
    
     environment {
    SONAR_TOKEN = credentials('sonar-token')
}

    stages {

        stage('Code checkout from GitHub') {
            steps {
                git branch: 'docker',
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
       stage('Deploy to Nexus') {
          steps {
            sh 'mvn clean deploy -DskipTests'
       }  
    }
        stage('Dockerize app') {
            steps {
                sh '''
                docker build -t achat .
                '''
            }
        }
        stage('Trivy Scan') {
            steps {
                sh '''
                docker run --rm \
                -v /var/run/docker.sock:/var/run/docker.sock \
                aquasec/trivy image achat
                '''
            }
        }
      stage('Deploy app') {
          steps {
            sh '''
             docker compose down -v || true
             docker compose up -d --build
             '''
       }
    }
      stage('OWASP ZAP Scan') {
           steps {
           sh '''
           docker run --rm -t owasp/zap2docker-stable \
           zap-baseline.py -t http://172.17.0.1:8089/SpringMVC/v3/api-docs
        '''
    }
}
   }
}
