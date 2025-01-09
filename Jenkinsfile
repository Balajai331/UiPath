pipeline {
    agent any

    stages {
        
        stage('Install Platform') {
            steps {
                UiPathInstallPlatform(traceLevel: 'Verbose')
            }
        }

        stage('Pack') {
            steps {
                UiPathPack(
                    disableBuiltInNugetFeeds: false,
                    outputPath: "${WORKSPACE}\\Output",
                    projectJsonPath: "${WORKSPACE}\\src\\project.json",
                    traceLevel: 'Verbose',
                    version: AutoVersion()
                )
            }
        }

        stage('Deploy') {
            steps {
                UiPathDeploy(
                    createProcess: true,
                    credentials: ExternalApp(
                        accountForApp: 'qbotitiqbbux',
                        applicationId: '545b32c5-558b-4024-aab5-e4032da1f2a8',
                        applicationScope: 'OR.Assets OR.Folders OR.BackgroundTasks OR.TestSets OR.TestSetExecutions OR.TestSetSchedules OR.Settings.Read OR.Robots.Read OR.Machines.Read OR.Execution OR.Users.Read OR.Jobs OR.Monitoring',
                        applicationSecret: '545b32c5-558b-4024-aab5-e4032da1f2a8',
                        identityUrl: '' // Add the valid identity URL if required
                    ),
                    entryPointPaths: 'Main.xaml',
                    folderName: 'ProcessTest_CICD',
                    orchestratorAddress: 'https://cloud.uipath.com/',
                    orchestratorTenant: 'CICD',
                    packagePath: "${WORKSPACE}\\Output",
                    environments: 'Development', // Specify the environment(s) here
                    traceLevel: 'Verbose'
                )
            }
        }
    }
}
