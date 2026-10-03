pipeline {
    agent any
    
    options {
        ansiColor('xterm')
    }

    stages {
        stage('build') {
            steps {
                // Dynamically strip the 'C:' and format the workspace for Docker Desktop
                script {
                    def linuxPath = pwd().replace('\\', '/').replaceAll('^[a-zA-Z]:', '')
                    
                    // We call raw 'bat' to execute the container manually 
                    bat "docker run --rm -v /c${linuxPath}:/app -w /app node:22-alpine sh -c \"npm ci && npm run build\""
                }
            }
        }

        stage('test') {
            steps {
                script {
                    def linuxPath = pwd().replace('\\', '/').replaceAll('^[a-zA-Z]:', '')
                    
                    bat "docker run --rm -v /c${linuxPath}:/app -w /app node:22-alpine sh -c \"npx vitest run --reporter=verbose\""
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
