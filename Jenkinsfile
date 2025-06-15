pipeline {
    agent any

    environment{
        NETLIFY_SITE_ID = "899514f8-f025-4e36-b7d5-292d85caeabc"
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
}

    stages {
    
        stage('Build') {
            agent{
                docker{
                    image "node:18-alpine"
                    reuseNode true
                }
            }
            steps {
                echo 'Hello World'
                sh'''
                ls -la
                node --version
                npm --version
                npm ci
                npm run build
                ls -la
                '''
            }
        }
        stage('Test') {
            steps{
             sh'''
             test -f "build/index.html"
             '''
            } 
}
        stage('Deploy'){
             agent{
                docker{
                    image "node:18-alpine"
                    reuseNode true
                }
            }
            steps{
                sh'''
                npm install netlify-cli@20.1.1
                node_modules/.bin/netlify --version
                echo "my site id is $NETLIFY_SITE_ID" 
                node_modules/.bin/netlify status
                 node_modules/.bin/netlify deploy --dir=build --prod
                '''
            }
}
}
}
