pipeline {
    agent any
    environment {
        IMAGE_NAME = "salmabm45/timesheet-salmabenmiled-2lainfo1"
        IMAGE_TAG  = "1.1"
        NAMESPACE  = "chap4"
        DEPLOYMENT = "timesheet-dep"
        CONTAINER  = "timesheet"
        KUBECONFIG = "/var/lib/jenkins/.kube/config"
    }
    stages {
        stage('GIT') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/salmabm/timesheet-project.git'
            }
        }
        stage('COMPILATION') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('INSTALLATION') {
            steps {
                sh """
                    docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                    docker login -u salmabm45 -p salma++123
                    docker push ${IMAGE_NAME}:${IMAGE_TAG}
                """
            }
        }
        stage('DEPLOIEMENT') {
            steps {
                sh """
                    kubectl set image deployment/${DEPLOYMENT} \
                        ${CONTAINER}=${IMAGE_NAME}:${IMAGE_TAG} \
                        -n ${NAMESPACE} \
                        --kubeconfig=${KUBECONFIG}
                """
            }
        }
    }
}
