pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }
  stages {



    
  
          stage('Docker Bench Security') {
      steps {
        sh 'chmod +x docker-bench-security.sh'
        sh './docker-bench-security.sh'
      }
    }


     stage('SonarQube Analysis') {
          agent any
      steps {
       

       sh '/var/opt/sonar-scanner-4.7.0.2747-linux/bin/sonar-scanner -Dsonar.projectKey=recommendationservice -Dsonar.sources=. -Dsonar.host.url=http://172.31.7.193:9000 -Dsonar.token=sqp_08f3aac8a963809f89bc559bb3cef46e31001dae'

        
      }
    }





    stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage = docker.build('koushiksai/recommendationservice:latest', '.')
                }
            }
        }


    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push koushiksai/recommendationservice'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
