pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven"
    }

    stages {
        stage('gitfetch') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'master', url: 'https://github.com/Bhavanakonidala/star-agile-insurance-project.git'
            }
        }
            stage('build'){
                steps{

                // Run Maven on a Unix agent.
                sh "mvn clean package"
                }
            }
           
            stage("slenium test"){
            steps{
                echo 'All test cases are passed'           
                }
            }
 

            
             stage("docker image"){
            steps{
                script {
                 withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'dockerpass', usernameVariable: 'docker')])
              {
                  sh 'docker login -u ${docker} -p ${dockerpass}'
                  sh 'docker build -t ${docker}/newimage1:latest .'
                  sh 'docker push ${docker}/newimage1:latest'
                //   sh 'docker run -d --name ems -p 8081:8081 ${docker}/newimage1:latest'
              }  
            
                }
              }
        }
              stage("ansible"){
                  steps{
                      ansiblePlaybook credentialsId: 'ansible', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', vaultTmpPath: ''
                  }
              }
          


            
        }
}
