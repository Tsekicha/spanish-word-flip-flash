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
    }
}