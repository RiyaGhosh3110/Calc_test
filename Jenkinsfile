pipeline {
    agent any
    environment {
        imageName = ""
    }
    stages {
        stage('Step 1 Git') {
            steps {
                git 'https://github.com/RiyaGhosh3110/Calc_test.git'
            }
        }
         stage('Step 2 Maven') {
            steps {
                 sh 'mvn clean install'
            }
        }
         stage('Step 3 Test')
         {
             steps {
                 sh 'mvn test'
             }
             post{
                always {
                    junit '**/target/surefire-reports/TEST-*.xml'
                }
             }
         }

         stage('Step 4 Docker_Image')
          {
              steps {
                  script {
                    imageName = docker.build "riyaghosh3110/test:latest"
                    }
              }
          }

         stage('Step 5 Push Docker Image')
        {
            steps {
                script{
                  docker.withRegistry('', 'docker-credentials') {
                       imageName.push()
                  }
                }
            }
        }
        stage('Step 6 Ansible'){
            steps{
                ansiblePlaybook becomeUser: null, colorized: true, disableHostKeyChecking: true, installation: 'Ansible', inventory: 'deploy-docker/inventory', playbook: 'deploy-docker/deploy-image.yml', sudoUser: null
            }
        }
    }
}

