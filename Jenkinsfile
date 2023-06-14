pipeline {
  agent any
  environment {
    dockerimagename = "kaazim/train-schedule"
  }
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                bat './gradlew build'
                bat './gradlew npm_start'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }

    stage('Checkout Source') {
      steps {
        git 'https://github.com/hereazim/cicd-pipeline-train-schedule-autodeploy.git'
      }
    }

    stage('Build  Docker Image') {
      steps{
        script {
          app = docker.build dockerimagename
        }
      }
    }
     stage('Push Docker Image'){
            steps{
                script{
                    bat 'docker login -u kaazim -p Azimka@01#'
                    bat 'docker push kaazim/train-schedule'
                }
            }
        }
  stage('CanaryDeploy') {
    steps{
            environment { 
                CANARY_REPLICAS = 1
            }
    }
  }
    stage('Deploying  container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(            
                    configs: 'train-schedule-kube-canary.yml',
                    enableConfigSubstitution: true)
        }
      }
    }

  }

}
