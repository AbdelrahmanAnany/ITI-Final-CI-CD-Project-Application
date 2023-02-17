pipeline {
    agent any

    stages {

        stage('CI'){
            steps {

                withCredentials([usernamePassword(credentialsId: 'Dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')])            {

                sh """
                    docker build -t 3anany/helloworld:v$BUILD_NUMBER .
                    docker login -u ${USERNAME} -p ${PASSWORD}
                    docker push 3anany/helloworld:v$BUILD_NUMBER

                """
                }
              }
        }
    

        stage('CD'){
            steps {

                withCredentials([file(credentialsId: 'AccessCluster', variable: 'config')]){
                    sh """
                        gcloud auth activate-service-account --key-file=${config}
                        gcloud container clusters get-credentials my-gke-cluster --region us-central1 --project iti-abdelrahman
                        sed -i 's/tag/${BUILD_NUMBER}/g' deployment.yml
                        kubectl apply -f deployment.yml
                    """
                }
            }
 
        }
    }    
}



