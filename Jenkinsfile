pipeline{
    agent any
   stages{
             stage ('build & Test')
       {
            steps{
                sh "mvn clean install"
            }
        }
             stage('Sonar') 
         {
             environment {
               scannerHome=tool 'sonar scanner'
       }
                steps {
                sh "mvn sonar:sonar -Dsonar.host.url=http://3.14.251.87:9000"
            }
         }
        
               stage ('Uploading  to nexus'){
                  
                   steps{
             withCredentials([usernamePassword(credentialsId: 'Bizeet_nexus', passwordVariable: 'pass', usernameVariable: 'usr')]) 
                 {
    sh   'curl -u $usr:$pass --upload-file target/springapp-${BUILD_NUMBER}.war http://3.14.251.87:8081/nexus/content/repositories/devopstraining/Ashish_spw/springapp-${BUILD_NUMBER}.war'
}           
        }
         }
  
         stage ('Deploy'){
            steps{
                 withCredentials([usernamePassword(credentialsId: 'devops-tomcat', passwordVariable: 'pass', usernameVariable: 'userId')]) 
                {
        
                   
                     sh 'curl -u $userId:$pass http://ec2-18-224-182-74.us-east-2.compute.amazonaws.com:8080/manager/text/undeploy?path=/Ashish_DEMO'
                    sh 'curl -u  $userId:$pass --upload-file target/springapp-${BUILD_NUMBER}.war http://ec2-18-224-182-74.us-east-2.compute.amazonaws.com:8080/manager/text/deploy?config=file:/var/lib/tomcat8/springapp-${BUILD_NUMBER}.war\\&path=/Ashish_DEMO'
            }
        }
         }
       /*stage ('Notify'){
           steps{
        def jsonString = '{"jobname":"${env.JOB_NAME}","buildurl":"${env.BUILD_URL}","buildnumber":"${env.BUILD_NUMBER}"}'
        def jsonObj = readJSON text: jsonString
        //assert jsonObj['jobname'] == 'katone'  // this is a comparison.  It returns true
        sh "echo ${jsonObj.jobname}"  // prints out jobname
        sh "echo ${jsonObj.buildurl}"   // prints out url
        sh "echo ${jsonObj.buildnumber}"   // prints out no.
           }
       }*/
       
   }
post {
    always {
        def jsonString = '{"jobname":"${env.JOB_NAME}","buildurl":"${env.BUILD_URL}","buildnumber":"${env.BUILD_NUMBER}"}'
        def jsonObj = readJSON text: jsonString

        //assert jsonObj['jobname'] == 'katone'  // this is a comparison.  It returns true
        sh "echo ${jsonObj.jobname}"  // prints out jobname
        sh "echo ${jsonObj.buildurl}"   // prints out url
        sh "echo ${jsonObj.buildnumber}"   // prints out no.
    }
    
}
}

