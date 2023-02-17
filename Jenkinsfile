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

                withCredentials([file(credentialsId: 'gcpCredential', variable: 'config')]){
                    sh """
                        gcloud auth activate-service-account --key-file=${config}
                        gcloud container clusters get-credentials my-gke-cluster --region us-central1 --project iti-abdelrahman
                        sed -i 's/tag/${BUILD_NUMBER}/g' deployment.yaml
                        kubectl apply -f deployment.yaml
                    """
                }
            }
 
        }
    }    
}



