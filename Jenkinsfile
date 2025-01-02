pipeline {
    agent any

    // Environment Variables
    environment {
        MAJOR = '1'
        MINOR = '0'
        // Orchestrator Services
        UIPATH_ORCH_URL = "https://cloud.uipath.com/"
        UIPATH_ORCH_LOGICAL_NAME = "qBotitiqbbux"
        UIPATH_ORCH_TENANT_NAME = "CICD"
        UIPATH_ORCH_FOLDER_NAME = "ProcessTest_CICD"
    }

    // Options for the pipeline
    options {
        // Timeout for pipeline execution
        timeout(time: 80, unit: 'MINUTES')
        skipDefaultCheckout()
    }

    stages {
        // Printing Basic Information
        stage('Preparing') {
            steps {
                echo "Jenkins Home: ${env.JENKINS_HOME}"
                echo "Jenkins URL: ${env.JENKINS_URL}"
                echo "Jenkins JOB Number: ${env.BUILD_NUMBER}"
                echo "Jenkins JOB Name: ${env.JOB_NAME}"
        
            }
        }

        // Building Tests
        stage('Build Tests') {
            steps {
                echo "Building package with ${WORKSPACE}"
                UiPathPack(
                    outputPath: "Output/${env.BUILD_NUMBER}",
                    outputType: 'Tests',
                    projectJsonPath: "project.json",
                    version: [
                        $class: 'ManualVersionEntry',
                        version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"
                    ],
                    useOrchestrator: false,
                    traceLevel: 'None'
                )
            }
        }

        // Deploying Tests
        stage('Deploy Tests') {
            steps {
                echo "Deploying ${env.BRANCH_NAME} to orchestrator"
                UiPathDeploy(
                    packagePath: "Output/Tests/${env.BUILD_NUMBER}",
                    orchestratorAddress: "${UIPATH_ORCH_URL}",
                    orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
                    folderName: "${UIPATH_ORCH_FOLDER_NAME}",
                    environments: 'INT',
                    credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: 'APIUserKey'),
                    traceLevel: 'None',
                    entryPointPaths: 'Main.xaml',
                    createProcess: true // Mandatory parameter added
                )
            }
        }
    }

    // Post-build actions
    post {
        success {
            echo 'Deployment has been completed!'
        }
        failure {
            echo "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.JOB_DISPLAY_URL})"
        }
        always {
            // Clean workspace
            cleanWs()
        }
    }
}
