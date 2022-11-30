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

        stage('K8S Manifest Update') {

            steps {

                git credentialsId: 'eunbin0523', 
                    url: 'https://github.com/eunbin0523/shopping_argo.git',
                    branch: 'master'

                sh "sed -i 's/springshoppingcart:.*\$/springshoppingcart:${env.BUILD_NUMBER}/g' deploysvc.yaml"
                sh "git add deploysvc.yaml"
                sh "git commit -m '[UPDATE] springshoppingcart ${env.BUILD_NUMBER} image versioning'"

                withCredentials([gitUsernamePassword(credentialsId: 'eunbin0523')]) {
                    sh "git push -u origin main"
                }
            }
            post {
                    failure {
                    echo 'Update failure !'
                    }
                    success {
                    echo 'Update success !'
                    }
            }
        }

    }             

}
