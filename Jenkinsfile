pipeline {
    environment {
        PASSWORD = credentials('dockerhubpass')
    }
    agent any

    stages {

        stage('Make scripts executable') {
            steps {
                sh 'chmod +x ./scripts/*.sh'
            }
        }

        stage('Run before-installation.sh') {
            steps {
                sh './scripts/before_installation.sh'
            }
        }

        stage('Build images') {
            steps {
                sh './scripts/build_images.sh'
            }
        }

        stage('Docker Push') {
            steps {
                sh "docker login -u samuel240210 -p ${'PASSWORD'}"
                sh 'docker push samuel240210/petclinic-fe:latest'  
            }
        }

        stage('Deploy Kubernetes') {
            steps {
                sh './scripts/kubernetes_deploy.sh'
            }
        }

        stage('Run after installation script') {
            steps {
                sh './scripts/after_installation.sh'
            }
        }

    }

}
