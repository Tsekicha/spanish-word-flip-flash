pipeline {
    agent any
    
    options {
        ansiColor('xterm')
    }

    stages {
        stage('build & test') {
            agent {
                docker {
            image 'node:22-alpine'
                }
             }
    steps {
        sh 'npm ci'
        sh 'npm run build'
        sh 'npx vitest run --reporter=verbose'
    }
}

        stage('test') {
            parallel {
                stage('unit tests') {
                    agent {
                        docker {
                    image 'node:22-alpine'
                        reuseNode true
                    }
                }
                    steps {
                        // Install dependencies first
                        sh 'npm ci'

                        // Then run Vitest
                        sh 'npx vitest run --reporter=verbose'    
                    }
                }
                stage("integration tests"){
                    agent{
                        docker{
                            image 'mcr.microsoft.com/playwright:v1.58.2-jammy'
                            reuseNode true
                            alwaysPull true
                        }
                    }
                    steps{

                        sh 'npm ci'
                        sh 'npx playwright test'
                    }
                }
            }
        }

        stage('deploy') {
            agent {
                docker {
                    image 'alpine'
                }
            }
            steps {
                // Mock deployment which does nothing
                echo 'Mock deployment was successful!'
            }
        }

         stage('e2e') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.58.2-jammy'
                    reuseNode true
                }
            }
            steps {
                sh 'npx playwright test'
            }
        }
    }
}