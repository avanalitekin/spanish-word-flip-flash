pipeline {
    agent any
    
    options {
        ansiColor('xterm')
    }

    stages {
        stage('build') {
            steps {
                script {
                    // 1. Transform Windows path (C:\Users\...) to a Docker-friendly Linux format (/Users/...)
                    def dockerVolPath = pwd().replace('\\', '/').replaceAll('^[a-zA-Z]:', '')
                    
                    echo "Mounting volume: /c${dockerVolPath}"
                    
                    // 2. Run the container manually with a true Linux absolute working directory (-w /app)
                    bat "docker run --rm -v /c${dockerVolPath}:/app -w /app node:22-alpine sh -c \"npm ci && npm run build\""
                }
            }
        }

        stage('test') {
            parallel {
                stage('unit tests') {
                    steps {
                        script {
                            def dockerVolPath = pwd().replace('\\', '/').replaceAll('^[a-zA-Z]:', '')
                            
                            // Runs vitest inside the container
                            bat "docker run --rm -v /c${dockerVolPath}:/app -w /app node:22-alpine sh -c \"npx vitest run --reporter=verbose\""
                        }
                    }
                }
            }
        }

        stage('deploy') {
            steps {
                // Mock deployment which does nothing
                echo 'Mock deployment was successful!'
            }
        }
    }
}