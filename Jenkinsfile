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
                stage('unit tests') {
                    steps {
                        script {
                            docker.image('mcr.microsoft.com/playwright:v1.57.0-jammy').inside {
                                sh 'npm ci'
                                sh 'npm run test:e2e -- --project=chromium'
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
}