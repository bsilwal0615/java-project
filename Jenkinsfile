properties([pipelineTriggers([githubPush()])])

node('linux') {  
    stage('Unit Tests') {  
         git 'https://github.com/bsilwal0615/java-project.git'
         sh 'ant -f test.xml -v'
         junit 'reports/result.xml'
    } 
    stage('Build') {    
          sh 'ant -f build.xml -v'   
    }
    stage('Deploy') {
         sh 'aws s3 cp /workspace/java-pipeline/dist/*.jar s3://seis665hw10/'
    }
    stage ('Report'){  
         withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'de987d89-8a98-4cc0-aaf8-1e6d182a3a70', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
         sh 'aws cloudformation describe-stack-resources --stack-name jenkins --region us-east-1'  
       }
    }
}
