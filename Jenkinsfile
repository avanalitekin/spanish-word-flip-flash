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
                    
                    // Changed to a named volume: -v spanish-word-flip-modules:/app/node_modules
                    bat "docker run --rm -v /c${dockerVolPath}:/app -v spanish-word-flip-modules:/app/node_modules -w /app node:22-alpine sh -c \"npm ci && npm run build\""
                }
            }
        }

        stage('test') {
            parallel {
                stage('unit tests') {
                    steps {
                        script {
                            def dockerVolPath = pwd().replace('\\', '/').replaceAll('^[a-zA-Z]:', '')
                            
                            // Re-using the exact same named volume here grants this container access to the dependencies
                            bat "docker run --rm -v /c${dockerVolPath}:/app -v spanish-word-flip-modules:/app/node_modules -w /app node:22-alpine sh -c \"npx vitest run --reporter=verbose\""
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