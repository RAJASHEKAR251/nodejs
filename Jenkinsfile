pipeline {
    agent any

    stages {
        stage ('Clone') {
            steps {
                git branch: 'main', url: "https://github.com/RAJASHEKAR251/nodejs.git"
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "artifactory-server-id",
                    url: "http://54.205.236.32:8082/",
                    credentialsId: "jfrog"
                )

                
                rtNpmDeployer (
                    id: "NPM_DEPLOYER",
                    serverId: "artifactory-server-id",
                    repo: "nodejs"
                )
            }
        }

        stage ('Exec npm install') {
            steps {
                rtNpmInstall (
                    tool: "nodejs-17.8.0", // Tool name from Jenkins configuration
                    
                )
            }
        }

        stage ('Exec npm publish') {
            steps {
                rtNpmPublish (
                    tool: "nodejs-17.8.0", // Tool name from Jenkins configuration
                    path: "npm-example",
                    deployerId: "NPM_DEPLOYER"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "artifactory-server-id"
                )
            }
        }
    }
}
