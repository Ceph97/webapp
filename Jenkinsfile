pipeline{
    //Directives
    agent any
    tools {
        maven 'Maven'
    }

    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }

        // Stage3 : Publish the source code to Sonarqube
        // stage ('Sonarqube Analysis'){
        //     steps {
        //         echo ' Source code published to Sonarqube for SCA......'
        //         withSonarQubeEnv('sonarqube'){ // You can override the credential to be used
        //              sh 'mvn sonar:sonar'
        //         }

        //     }
        // }

        // Stage3: Publish the source code to Nexus
        stage ('Publish to Nexus'){
            steps {
                    nexusArtifactUploader artifacts: [[artifactId: 'VinayDevOpsLab', classifier: '', file: 'target/VinayDevOpsLab-0.0.4-SNAPSHOT.war', type: 'war']], credentialsId: '713b71cb-6a1a-419b-ba8b-1530d384d211', groupId: 'com.vinaysdevopslab', nexusUrl: 'ec2-3-81-84-101.compute-1.amazonaws.com:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'CephDevopsLab-SNAPSHOT', version: '0.0.4-SNAPSHOT'
            }
        }
    

        // Stage3: Deploy
        stage ('Deploy'){
            steps {
                echo ' Deploying the application......'
            }
        }

        
        
    }

}