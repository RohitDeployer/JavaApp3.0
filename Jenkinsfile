@Library('Shared_lib') _

pipeline{

    agent any

    parameters{

        choice(name: 'action', choices: 'create\ndelete', description: 'Choose create/Destroy')
        string(name: 'ImageName', description: "name of the docker build", defaultValue: 'javapp')
        string(name: 'ImageTag', description: "tag of the docker build", defaultValue: 'v1')
        string(name: 'DockerHubUser', description: "name of the Application", defaultValue: 'rohtmore007')
    }

    stages{
         
        stage('Git Checkout'){
                    when { expression {  params.action == 'create' } }
            steps{
            gitCheckout(
                branch: "main",
                url: "https://github.com/RohitDeployer/JavaApp3.0.git"
            )
            }
        }
        stage('Unit Test maven'){
         
         when { expression {  params.action == 'create' } }

            steps{
               script{
                   
                   mvnTest()
               }
            }
        }
        stage('Integration Test maven'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   mvnIntegrationTest()
               }
            }
        }
<<<<<<< HEAD
        stage('Static code analysis: Sonarqube'){
         when { expression {  params.action == 'create' } }
=======
         stage('Static code analysis: Sonarqube'){
          when { expression {  params.action == 'create' } }
>>>>>>> 5c373790d6f15d80e4235493617931e86032af78
             steps{
                script{
                   
                    def SonarQubecredentialsId = 'sonarqube-api'
                    statiCodeAnalysis(SonarQubecredentialsId)
                }
<<<<<<< HEAD
            }
        }
        stage('Quality Gate Status Check : Sonarqube'){
         when { expression {  params.action == 'create' } }
=======
             }
        }
        stage('Quality Gate Status Check : Sonarqube'){
          when { expression {  params.action == 'create' } }
>>>>>>> 5c373790d6f15d80e4235493617931e86032af78
             steps{
                script{
                   
                    def SonarQubecredentialsId = 'sonarqube-api'
                    QualityGateStatus(SonarQubecredentialsId)
                }
<<<<<<< HEAD
            }
=======
             }
>>>>>>> 5c373790d6f15d80e4235493617931e86032af78
        }
        stage('Maven Build : maven'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   mvnBuild()
               }
            }
        }
        stage('JFrog Artifactory Upload') {
            when { expression { params.action == 'create' } }
            steps {
                script {
                    def artifactoryConfig = [
                        serverId: 'artifactory', // Set this to the configured server ID in Jenkins
                        spec: """{
                            "files": [
                                {
                                    "pattern": "target/*.jar",
                                    "target": "JavaAppArtifacts/${params.ImageName}/${params.ImageTag}/"
                                }
                            ]
                        }""",
                        buildName: "${params.ImageName}",
                        buildNumber: "${params.ImageTag}"
                    ]

                    artifactoryUtils(artifactoryConfig)
                }
            }
        }
        stage('Docker Image Build'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   dockerBuild("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
               }
            }
        }
        stage('Docker Image Scan: trivy '){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   dockerImageScan("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
               }
            }
        }
        stage('Docker Image Push : DockerHub '){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   dockerImagePush("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
               }
            }
        }   
        stage('Docker Image Cleanup : DockerHub '){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   dockerImageCleanup("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
               }
            }
        }      
    }
}
