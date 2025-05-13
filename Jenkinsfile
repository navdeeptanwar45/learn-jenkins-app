pipeline {
    agent any

    environment {
    NETLIFY_SITE_ID = "b40c6866-28f4-4c39-b0fb-f250485e05d9"
    NETLIFY_AUTH_TOKEN = credentials("netlify-token")
           }


    stages {
        stage('build') {
            agent{
                docker{
                    image "node:18-alpine"
                    reuseNode true
                }
            }
            steps {
               sh '''
               ls -la 
               node --version
               npm --version
               npm ci 
               npm run build
               ls -la
               '''

            }
        }
        stage('test'){
             agent{
                docker{
                    image "node:18-alpine"
                    reuseNode true
                }
            }
            steps{
                sh '''
                npm test
                '''
            }
        }
        stage('deploy') {
            agent{
                docker{
                    image "node:18"
                    reuseNode true
                }
            }
            steps {
               sh '''
            npm install netlify-cli 
           node_modules/.bin/netlify --version
           node_modules/.bin/netlify status
           node_modules/.bin/netlify deploy --dir=build --prod

               '''

            }
        }
    }
    post{
        always{
            junit "test-results/junit.xml"
        }
    }
}