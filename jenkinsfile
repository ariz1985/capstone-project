pipeline {
    agent any

    tools {
        maven "M2"
    }

    stages {
        stage('Git Login') {
            steps {
               git 'https://github.com/ariz1985/capstone-project.git'
            }
        }
        stage('Maven Build'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('Publish HTML Report'){
            steps{
                publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/var/lib/jenkins/workspace/capstone-project/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
            }
        }
        stage('Docker Build'){
            steps{
                sh 'docker build -t ariz1985/insure-me1.0 .'
            }
        }
        stage('Docker Hub Login'){
            steps{
                withCredentials([usernamePassword(credentialsId: '6f4d93f9-9881-4f8a-a9a4-5b1b7ec04309', passwordVariable: 'docker_pass', usernameVariable: 'docker_usr')]) {
                    sh 'docker login -u $docker_usr -p $docker_pass'
                }
            }
        }
        stage('Docker Push'){
            steps{
                sh 'docker push ariz1985/insure-me1.0'
            }
        }
    }
}
