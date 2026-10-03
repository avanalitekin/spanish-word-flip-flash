pipeline {
    agent any
    
    options {
        ansiColor('xterm')
    }

    stages {
        stage('build') {
            steps {
                script {
                    def dockerVolPath = pwd().replace('\\', '/').replaceAll('^[a-zA-Z]:', '')
                    
                    // Notice the extra: -v /app/node_modules
                    // This isolates the slow npm operations inside Linux's local cache
                    bat "docker run --rm -v /c${dockerVolPath}:/app -v /app/node_modules -w /app node:22-alpine sh -c \"npm ci && npm run build\""
                }
            }
        }

        stage('test') {
            parallel {
                stage('unit tests') {
                    steps {
                        script {
                            def dockerVolPath = pwd().replace('\\', '/').replaceAll('^[a-zA-Z]:', '')
                            
                            // Re-apply the volume exception here as well so tests are fast
                            bat "docker run --rm -v /c${dockerVolPath}:/app -v /app/node_modules -w /app node:22-alpine sh -c \"npx vitest run --reporter=verbose\""
                        }
                    }
                }
            }
        }

        stage('deploy') {
            steps {
                echo 'Mock deployment was successful!'
            }
        }
    }
}