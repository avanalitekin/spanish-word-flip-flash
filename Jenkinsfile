pipeline {
    agent any
    
    options {
        ansiColor('xterm')
    }

    stages {
        stage('build') {
            steps {
                script {
                    // Strips the "C:" and switches to forward slashes dynamically
                    def linuxPath = pwd().replace('\\', '/').replaceAll('^[a-zA-Z]:', '')
                    
                    docker.image('node:22-alpine').inside("-v ${linuxPath}:${linuxPath} -w ${linuxPath}") {
                        sh 'npm ci'
                        sh 'npm run build'
                    }
                }
            }
        }

        stage('test') {
            steps {
                script {
                    def linuxPath = pwd().replace('\\', '/').replaceAll('^[a-zA-Z]:', '')
                    
                    docker.image('node:22-alpine').inside("-v ${linuxPath}:${linuxPath} -w ${linuxPath}") {
                        sh 'npx vitest run --reporter=verbose'
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
