pipeline {
    agent any
    stages{
        stage('Build Maven'){
            steps{
                git url:'https://github.com/mithun02/cicdmithun/', branch: "master"
               sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t mithun02/endtoendproject25may:v1 .'
                }
            }
        }
          stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u mithun02 --password-stdin"
                    sh 'docker push mithun02/endtoendproject25may:v1'
                }
            }
        }
        
        
        stage('Deploy to k8s'){
            when{ expression {env.GIT_BRANCH == 'master'}}
            steps{
                script{
                     kubernetesDeploy (configs: 'deploymentservice.yaml' ,kubeconfigId: 'k8sconfigpwd')
                   
                }
            }
        }
    }
}
