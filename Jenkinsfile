pipeline {

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
            agent any
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                    sh 'docker push shanem/spring-petclinic:latest
                }
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
