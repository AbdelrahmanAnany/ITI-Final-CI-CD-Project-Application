pipeline {
    agent any

    stages {

        stage('CI'){
            steps {

                withCredentials([usernamePassword(credentialsId: 'DockerHub-Credential', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')])            {

                sh """
                    cd app
                    docker build -t 3anany/helloworld:v$BUILD_NUMBER .
                    docker login -u ${USERNAME} -p ${PASSWORD}
                    docker push 3anany/helloworld:v$BUILD_NUMBER
                    cd ..

                """
                }
              }
        }

        stage('CD'){
            steps {

                withCredentials([file(credentialsId: 'gcpCredential', variable: 'GCLOUD_CREDS')]){
                    sh """
                        gcloud auth activate-service-account --key-file="$GCLOUD_CREDS"
                        apt get update && upgrade -y
                        apt-get remove kubectl
                        apt-get install kubectl
                        apt-get install google-cloud-sdk-app-engine-java kubectl -y
                        gcloud container clusters get-credentials private-cluster --zone us-central1-a --project iti-abdelrahman
                        sed -i 's/tag/${BUILD_NUMBER}/g' deployment.yaml
                        kubectl apply -f deployment.yaml
                    """
                }
            }
 
        }
    }    
}



