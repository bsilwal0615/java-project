properties([pipelineTriggers([githubPush()])])

node('linux') {  
    stage('Unit Tests') { 
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
         withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: '98039ec7-6d35-4a22-963f-076f9250619c', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
         sh 'aws cloudformation describe-stack-resources --stack-name jenkins --region us-east-1'  
       }
   }
}
