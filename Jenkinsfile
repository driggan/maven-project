pipeline {
    agent any
    tools {maven 'localMaven'
    }
    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.216.101.227', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.221.221.96', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sudo sh "scp -i Users/driggan/Downloads/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sudo sh "scp -i Users/driggan/Downloads/tomcat2.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }

}
