pipeline {
    agent any
    stages {
        stage("Prepare") {
            steps {
                echo "Echo Step"
                script {
                    print "Script Step"
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
                // build job: 'K8S_RECYCLE', parameters: [
                //     string(name: 'NAMESPACE', value: 'breach-engine'), 
                //     string(name: 'POD_SELECTION', value: 'web|worker')
                // ]
            }
        }
    }
}
