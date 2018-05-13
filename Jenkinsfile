dockerComposeFilePaths = ["docker-compose.yaml", "docker-compose.yml"]
pipeline {
    agent any
    stages {
        stage("Prepare") {
            steps {
                script {
                    dockerComposeFilePaths.each {
                        if (fileExists(it)) {
                            print "Compose file found"
                            composeFile = it
                        }
                    }
                }
            }
        }
        stage ("Recycle") {
            steps {    
                script {
                    ["breach-engine", "enterprise-portal"].each {
                        build([
                            job:"K8S_RECYCLE", 
                            parameters:[
                               string(name: 'NAMESPACE', value: "${it}"), 
                               string(name: 'POD_SELECTION', value: 'web|worker')
                            ]])
                    }
                }
            }
        }
    }
}
