pipeline
{
    agent any
    stages
    {
        stage('ContinuosDownload')
        {
            steps
            {
                script
                {
                    try
                    {
                        git 'https://github.com/krishnain/maven.git'    
                    }
                    catch(Exception e1)
                    {
                        mail bcc: '', body: 'Jenkins is unable to download the development code from remote github', cc: '', from: '', replyTo: '', subject: 'Download Failed', to: 'git.admin@gmail.com'
                        exit(1)
                    }
                }
               
            }
        }
        stage('ContinuosBuild')
        {
            steps
            {
                script
                {
                    try
                    {
                      sh 'mvn package'  
                    }
                    catch(Exception e2)
                    {
                      mail bcc: '', body: 'Jenkins is unable to create an artifact from the code', cc: '', from: '', replyTo: '', subject: 'BuildFailed', to: 'dev.team@gmail.com' 
                      exit(1)
                    }
                }
                
            }
        }
        stage('ContinuousDeployment')
        {
            steps
            {
                script
                {
                   try
                {
                     deploy adapters: [tomcat9(credentialsId: '172fbcf0-2887-4ec5-84df-85350ae3b106', path: '', url: 'http://172.31.25.176:8080')], contextPath: 'mytestapp', war: '**/*.war' 
                     
                }
                catch(Exception e3)
                {
                  mail bcc: '', body: 'Jenkins is unable to deploy into tomcat on QAServers', cc: '', from: '', replyTo: '', subject: 'Deployment Failed', to: 'mid.team@gmail.com'
                  exit(1)
                }
        }
                  
            }
        }
        stage('ContinuousTesting')
        {
            steps
            {
                script
                {
                    try
                    {
                        git 'https://github.com/selenium-saikrishna/FunctionalTesting.git'
                        sh 'java -jar /var/lib/jenkins/workspace/DeclarativePipeline/testing.jar'
                    }
                    catch(Exception e4)
                    {
                    mail bcc: '', body: 'Selenium scripts are failing', cc: '', from: '', replyTo: '', subject: 'Testing Failed', to: 'qa.team@gmail.com'    
                    exit(1)
                    }
                }
                
            }
        }
    }
}
