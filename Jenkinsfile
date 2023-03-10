pipeline{
    //Directives
    agent any
    tools {
        maven 'Maven'
    }

    environment {
        // Specify the environment variables
        // JAVA_HOME = '/usr/lib/jvm/java-8-openjdk-amd64'
        // PATH = "$JAVA_HOME/bin:$PATH"
        artifactId = readMavenPom().getArtifactId()
        version = readMavenPom().getVersion()
        name = readMavenPom().getName()
        groupId = readMavenPom().getGroupId()
        packaging = readMavenPom().getPackaging()
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
                script {
                    // This logic will make sure if version ends with SNAPSHOT then it will upload to snapshot repo
                    // else it will upload to release repo
                    def NexusRepo = version.endsWith("SNAPSHOT") ? "CephDevopsLab-SNAPSHOT" : "CephDevopsLab-RELEASE"

                    nexusArtifactUploader artifacts: 
                        [[artifactId: "${artifactId}",
                         classifier: '', 
                         file: "target/${artifactId}-${version}.${packaging}", 
                         type: 'war']], 
                         credentialsId: '713b71cb-6a1a-419b-ba8b-1530d384d211', 
                         groupId: "${groupId}", 
                         nexusUrl: 'ec2-3-81-84-101.compute-1.amazonaws.com:8081', 
                         nexusVersion: 'nexus3', 
                         protocol: 'http', 
                         repository: "${NexusRepo}", 
                         version: "${version}"
                }
            }
        }

        // Stage 4: to print the environment variables set above
        stage ('Print Environment Variables'){
            steps {
                echo "artifactId: ${artifactId}"
                echo "version: ${version}"
                echo "name: ${name}"
                echo "groupId: ${groupId}"
            }
        }

        // Stage 5: Deploying application using Ansible to Apache Tomcat
        stage ('Deploy'){
            steps {
                echo ' Deploying the application......'
                sshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: 'Ansible_controller', 
                            transfers: [
                                sshTransfer(
                                    cleanRemote: false, 
                                    excludes: '', 
                                    execCommand: 'ansible-playbook /opt/playbooks/downloadDeploy2.yaml -i /opt/playbooks/hosts', 
                                    execTimeout: 120000, 
                                    // flatten: false, 
                                    // makeEmptyDirs: false, 
                                    // noDefaultExcludes: false, 
                                    // patternSeparator: '[, ]+', 
                                    // remoteDirectory: '', 
                                    // remoteDirectorySDF: false, 
                                    // removePrefix: '', 
                                    // sourceFiles: ''
                                    )
                            ], 
                            usePromotionTimestamp: false, 
                            useWorkspaceInPromotion: false, 
                            verbose: false
                        )
                    ]
                )
            }
        }


        // Stage 5: Deploying to Docker
        stage ('Deploy to Docker'){
            steps {
                echo ' Deploying the application......'
                sshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: 'Ansible_controller', 
                            transfers: [
                                sshTransfer(
                                    cleanRemote: false, 
                                    excludes: '', 
                                    execCommand: 'ansible-playbook /opt/playbooks/downloadDeploy2.yaml -i /opt/playbooks/hosts', 
                                    execTimeout: 120000, 
                                    // flatten: false, 
                                    // makeEmptyDirs: false, 
                                    // noDefaultExcludes: false, 
                                    // patternSeparator: '[, ]+', 
                                    // remoteDirectory: '', 
                                    // remoteDirectorySDF: false, 
                                    // removePrefix: '', 
                                    // sourceFiles: ''
                                    )
                            ], 
                            usePromotionTimestamp: false, 
                            useWorkspaceInPromotion: false, 
                            verbose: false
                        )
                    ]
                )
            }
        }

        
        
    }

}