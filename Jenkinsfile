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
                withCredentials([usernamePassword(credentialsId: '236db16b-0abd-4570-942b-e8dbca3b72c7', passwordVariable: 'docker_pass', usernameVariable: 'docker_usr')]) {
                    sh 'docker login -u $docker_usr -p $docker_pass'
                }
            }
        }
        stage('Docker Push'){
            steps{
                sh 'docker push ariz1985/insure-me1.0'
            }
        }
        stage('Ansible Mgmt'){
            steps{
                ansiblePlaybook credentialsId: 'ansible-ssh', disableHostKeyChecking: true, installation: 'Ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible.yml', vaultTmpPath: ''
            }
        }
    }
}
