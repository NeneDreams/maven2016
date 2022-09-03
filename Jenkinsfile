pipeline
{
    agent any
    stages
    {
        stage('ContinuousDownload')
        {
            steps
            {
                script
                {
                    try
                    {
                        git 'https://github.com/NeneDreams/maven.git'
                    }
                    catch(Exception e1)
                    {
                        mail bcc: '', body: 'Jenkins is unable to download and deploy the code from github. ', cc: '', from: '', replyTo: '', subject: 'Download failed', to: 'gitad.admin@gmail.com'
                        exit(1)
                    }
                }
            }
        }
        stage('ContinuousBuild')
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
                        mail bcc: '', body: 'Jenkins has failed: Unable to create artifact from code', cc: '', from: '', replyTo: '', subject: 'Build Failed', to: 'dev.team@gmail.com'
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
                        deploy adapters: [tomcat9(credentialsId: 'd6879646-8da1-4f5d-88af-8cedd3d7619a', path: '', url: 'http://172.31.17.228:8080')], contextPath: 'testapp', war: '**/*.war'
                    }
                    catch(Exception e3)
                    {
                        mail bcc: '', body: 'Jenkins is unable to deploy into tomcat in the QAservers. ', cc: '', from: '', replyTo: '', subject: 'Deployment Failed', to: 'middleware.team@gmail.com'
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
                        git 'https://github.com/NeneDreams/Selenium.git'
                        sh 'java -jar /var/lib/jenkins/workspace/DeclarativePipeline/testing.jar'
                    }
                    catch(Exception e4)
                    {
                        mail bcc: '', body: 'Selenium script were failed', cc: '', from: '', replyTo: '', subject: 'Testing Failed', to: 'qa@gmail.com'
                        exit(1)
                        
                    }
                } 
            }
        }
        stage('ContinuousDelivery')
        {
            steps
            {
                script
                {
                    try
                    {
                        deploy adapters: [tomcat9(credentialsId: 'd6879646-8da1-4f5d-88af-8cedd3d7619a', path: '', url: 'http://172.31.19.155:8080')], contextPath: 'prodapp', war: '**/*.war' 
                    }
                    catch(Exception e5)
                    {
                        mail bcc: '', body: 'Jenkins in unable to deploy into tomcat9 in QAsservers.', cc: '', from: '', replyTo: '', subject: 'Delivery Failed', to: 'Deliveryteam@gmail.com'
                        exit(1)
                        
                    }
                } 
                
               
            }
        }
            
    }
}
