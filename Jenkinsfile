pipeline {
    agent any

    environment {
        MAJOR = '1'
        MINOR = '0'
        UIPATH_ORCH_URL = "https://cloud.uipath.com/"
        UIPATH_ORCH_LOGICAL_NAME = "qBotitiqbbux"
        UIPATH_ORCH_TENANT_NAME = "CICD"
        UIPATH_ORCH_FOLDER_NAME = "ProcessTest_CICD"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Preparing') {
            steps {
                echo "Jenkins Home: ${env.JENKINS_HOME}"
                echo "Jenkins URL: ${env.JENKINS_URL}"
                echo "Jenkins JOB Number: ${env.BUILD_NUMBER}"
                echo "Jenkins JOB Name: ${env.JOB_NAME}"
                echo "GitHub Branch Name: ${env.BRANCH_NAME}"
            }
        }
    }
}
