dockerComposeFilePaths = ["docker-compose.yaml", "docker-compose.yml"]
composeFile = null
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
                    if(composeFile != null) {
                        compose_content = readYaml([file:composeFile])
                        compose_content.services.each { service, svc_info -> 
                            print "Need to push service ${service}" 
                            image_name = svc_info.image.split('/')[1]
                            print "Image name ${image_name}"
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
