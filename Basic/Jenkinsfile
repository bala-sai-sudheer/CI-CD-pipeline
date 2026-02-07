pipeline{
    agent{
        label 'test'
    }
    tools{
        maven 'mymaven'
    }
    stages{
        stage("Code"){
            steps{
                git 'https://github.com/bala-sai-sudheer-p/one.git'
            }
        }
        stage("Reminding"){
            steps{
                script {
                    // Send email at the start of the build
                    emailext(
                        subject: "Build Started: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The build has started.",
                        to: 'pbssudheer143@gmail.com',
                    )
                }
            }
        }
        stage("Build&Test"){
            steps{
                sh 'mvn clean package'
            }
        }
        stage("Deploy"){
            steps{
                deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'tomcat-password', path: '', url: 'http://13.49.183.94:8080/')], contextPath: null, war: 'target/myweb-8.7.3.war'
            }
        }
    }
    post{
        always {
            // Use emailext for detailed, customizable emails
            emailext (
                subject: "Pipeline: ${env.job_name} - ${currentBuild.result}",
                body: """
                    <p>Build: ${currentBuild.displayName} (${currentBuild.number})</p>
                    <p>Status: ${currentBuild.result}</p>
                    <p>Build URL: ${env.BUILD_URL}</p>
                    <p>Check logs for details.</p>
                """,
                to: 'pbssudheer143@gmail.com', // Or use recipients: [developers()]
                // Attachments or other configs can go here
                attachLog: true
            )
        }
    }
}
