pipeline {
    agent  {
        label 'dind-agent'
    }
    stages {
        stage('Build image') {
            steps {
                script {
                    app = docker.build("eunoia0523/springshoppingcart")
                }
            }
        }
        
        stage("Push image to gcr") {
            steps {
                script {
                    docker.withRegistry('https://asia.gcr.io', 'gcr:eunoia0523') {
                        app.push("${env.BUILD_NUMBER}")
                    }
                }
            }
        }

    }             

}
