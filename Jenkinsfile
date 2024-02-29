pipeline{
     agent any
     
     stages{
          stage('Code'){
              steps{
                 git url: 'https://github.com/UtkanshAdlakha123/final-repo.git', branch: 'master'
                 echo 'cloning the code'
             }
         }
         
         stage('Copying content'){
             steps{
                 sh 'scp -r /var/lib/jenkins/workspace/ansible-jenkins-pipeline/* root@52.66.6.161:~/project/'
             }
         }
         
         stage('Build & Deploy'){
             steps{
                 sh 'ansible-playbook /var/lib/jenkins/playbooks/deployment.yaml '
             }
         }
         
     }
     
        post {
            always {
                emailext (
                    subject: "Jenkins Build status:  ${currentBuild.result}: ${env.JOB_NAME}",
                    body: "Build status:  ${currentBuild.result} for ${env.JOB_NAME}: ${env.BUILD_URL}",
                    to: "adlakhautkansh22@gmail.com",
                    from: "jenkins@example.com",
                    mimeType: "text/html"
                )
        }
    }       
 }
