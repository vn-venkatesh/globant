pipeline {
    agent any

    environment {
        // Define environment variables
        GOOGLE_APPLICATION_CREDENTIALS = credentials('GOOGLE_APPLICATION_CREDENTIALS')
        PROJECT_ID = 'globantproject-441713'                     //GCP Project ID
        CLUSTER_NAME = 'myprodcluster'                           //GKE Cluster Name
        ZONE = 'us-central1-a'                                   //Cluster Zone
    }

    stages {
        stage('Clone GitHub Repo') {
            steps {
                script {
                    git url: 'https://github.com/vn-venkatesh/my_mainfestfiles.git', branch: 'main'
                }
            }
        }
        
        stage('Authenticate with Google Cloud') {
            steps {
                script {
                    // Authenticate using the service account key
                    sh '''
                        export PATH=$PATH:/usr/local/google-cloud-sdk/bin
                        gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS
                        gcloud config set project $PROJECT_ID
                        gcloud config set compute/zone $ZONE
                    '''
                }
            }
        }

        stage('Get GKE Cluster Credentials') {
            steps {
                script {
                    // Get credentials for GKE cluster and configure kubectl
                    sh '''
                        export PATH=$PATH:/usr/local/google-cloud-sdk/bin
                        gcloud container clusters get-credentials $CLUSTER_NAME --zone $ZONE --project $PROJECT_ID
                    '''
                }
            }
        }

        stage('Deploy to GKE') {
            steps {
                script {
                    // Deploy your Kubernetes resources (e.g., deployment, service, etc.)
                    sh '''
                        export PATH=$PATH:/usr/local/google-cloud-sdk/bin
                        #deploying blue
                        #kubectl apply -f /var/lib/jenkins/workspace/01/manifest/blue.yaml
                        #deploying green
                        #kubectl apply -f /var/lib/jenkins/workspace/01/manifest/green.yaml
                        #patch switching to green
                        #kubectl patch ingress my-app-ingress --type='json' -p '[{"op": "replace", "path": "/spec/rules/0/http/paths/0/backend/service/name", "value": "green-service"}]'
                        #roll back to blue
                        kubectl patch ingress my-app-ingress --type='json' -p '[{"op": "replace", "path": "/spec/rules/0/http/paths/0/backend/service/name", "value": "blue-service"}]'
                    '''
                }
            }
        }
    }
}
