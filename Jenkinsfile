pipeline {
    agent any
    
    options {
        ansiColor('xterm')
    }

    stages {
        stage('build') {
            steps {
                script {
                    docker.image('node:22-alpine').inside {
                        sh 'npm ci'
                        sh 'npm run build'
                    }
                }
            }
        }

        stage('test') {
            parallel {
                stage('unit tests') {
                    steps {
                        script {
                            docker.image('node:22-alpine').inside {
                                sh 'npx vitest run --reporter=verbose'
                            }
                        }
                    }
                }
                stage('integration tests') {
                    steps {
                        script {
                            docker.image('mcr.microsoft.com/playwright:v1.57.0-jammy').inside {
                                sh 'npm ci'
                                sh 'npx playwright test'
                            }
                        }
                    }
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    docker.image('alpine').inside {
                        echo 'Mock deployment was successful!'
                        }
                    }
                }   
            }
    }
     post {
        always {     
            // Publish Playwright HTML report
            publishHTML([
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                icon: '',
                reportDir: 'reports-e2e/html/',
                reportFiles: 'index.html',
                reportName: 'Playwright HTML Report',
                keepAll: true
            ])
            
            // Publish JUnit XML results
            junit(
                testResults: 'reports-e2e/junit.xml',
                allowEmptyResults: true
            )
        }
    }
}