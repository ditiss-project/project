pipeline{
     agent any
         tools{
        jdk 'jdk11'
    }
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    stages {
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        stage('Checkout from Git'){
            steps{
                git branch: 'master', url: 'https://github.com/ditiss-project/project.git'
            }
        }
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=anon \
                    -Dsonar.projectKey=anon '''
                }
            }
        }
        
        stage('OWASP FS SCAN') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DP-Check'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
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
                    to: "aniketdahake050@gmail.com",
                    from: "jenkins@example.com",
                    mimeType: "text/html"
                )
        }
    }       
 }
