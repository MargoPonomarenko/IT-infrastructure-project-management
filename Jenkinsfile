pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/MargoPonomarenko/IT-infrastructure-project-management.git', credentialsId: 'github'
            }
        }

        stage('Copy PDB files') {
            steps {
                bat 'copy x64\\Debug\\*.pdb x64\\Release\\'
            }
        }
        
        stage('Build') {
            steps {
                bat '"C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Community\\MSBuild\\Current\\Bin\\MSBuild.exe" test_repos.sln /t:Build /p:Configuration=Release /p:PlatformToolset=v142 /p:WindowsTargetPlatformVersion=10.0.19041.0'
            }
        }

        stage('Test') {
            steps {
                // Команди для запуску тестів
                bat "x64\\Debug\\test_repos.exe --gtest_output=xml:test_report.xml"
            }
        }

        stage('Publish Test Results') {
            steps {
                bat 'copy test_report.xml **/test-results.xml'
                junit '**/test-results.xml'
            }
        }
    }

    post {
    always {
        // Publish test results using the junit step
         // Specify the path to the XML test result files
         junit 'x64\\Debug\\test_report.xml'
    }
}
}