pipeline {
  agent any

    // environment {
    //     NETLIFY_SITE_ID = 'bcbf9736-1e16-45ea-8e76-29ea1840719a'
    //     NETLIFY_AUTH_TOKEN = credentials('assign2Token')
    // }

  stages {
    // stage('Docker'){
    //   steps{
    //       sh 'docker build =t my-docker-image .'
    //   }
    // }

    // stage('Build') {
    //   agent {
    //     docker {
    //       image 'node:22.14.0-alpine'
    //       reuseNode true
    //     }
    //   }
    //   steps {
    //     sh '''
    //       ls -la
    //       node --version
    //       npm --version
    //       npm install
    //       npm run build
    //       ls -la
    //     '''
    //   }
    // }

    // stage('Test') {
    //   agent {
    //     docker {
    //       image 'node:22.14.0-alpine'
    //       reuseNode true
    //     }
    //   }
    //   steps {
    //     sh '''
    //       test -f build/index.html
    //       npm test
    //     '''
    //   }
    // }
    
    // stage('Deploy') {
    //     agent {
    //         docker {
    //             image 'node:22.14.0-alpine'
    //             reuseNode true
    //         }
    //     }
    //     steps {
    //         sh '''
    //         npm install netlify-cli
    //         node_modules/.bin/netlify --version
    //         echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
    //         node_modules/.bin/netlify status
    //         node_modules/.bin/netlify deploy --prod --dir=build
    //         '''
    //   }
    // }

    
    stage('Build') {
      agent {
        docker {
          image 'node:22.14.0-alpine'
          reuseNode true
        }
      }
      steps {
        sh '''
          ls -la
          node --version
          npm --version
          npm install
          npm run build
          ls -la
        '''
      }
    }

    stage('Test') {
      agent {
        docker {
          image 'node:22.14.0-alpine'
          reuseNode true
        }
      }
      steps {
        sh '''
          test -f build/index.html
          npm test
        '''
      }
    }


        stage('AWS') {
        agent {
            docker {
                image 'amazon/aws-cli'
                reuseNode true
                args '--entrypoint=""'
            }
        }

        environment {
          AWS_S3_BUCKET = 'temp-20250404'
        }
        
        steps {
          withCredentials([usernamePassword(credentialsId: 'my-temp', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
             sh '''
              aws --version
              aws s3 ls
              echo "Hello S3!" > index.html
              # aws s3 cp index.html s3://temp-20250404/index.html
              aws s3 sync build s3://$AWS_S3_BUCKET
            '''
          }
      }
    }
  }
}
