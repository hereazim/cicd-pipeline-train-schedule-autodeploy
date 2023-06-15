pipeline {
  agent any
  environment {
    dockerimagename = ""
  }
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                bat './gradlew clean'
                bat './gradlew assemble'
                bat './gradlew npm_start'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }

    stage('Checkout Source') {
      steps {
        git 'https://github.com//cicd-pipeline-train-schedule-autodeploy.git'
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
                    bat 'docker login -u '
                    bat 'docker push e'
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
