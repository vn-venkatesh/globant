pipeline {
    agent any

    environment {
        // Define environment variables
        GOOGLE_CREDENTIALS = credentials('gke-service-account')  // Jenkins credential ID
        PROJECT_ID = 'globantproject-441713'                       // Your GCP Project ID
        CLUSTER_NAME = 'myprodcluster'                       // Your GKE Cluster Name
        ZONE = 'us-central1-a'                               // GKE Cluster Zone
        DEPLOYMENT_FILE = 'k8s/deployment.yaml'                  // Path to your Kubernetes manifest
    }

    stages {
        stage('Authenticate with Google Cloud') {
            steps {
                script {
                    // Authenticate using the service account key
                    sh '''
                        echo "$GOOGLE_CREDENTIALS" > gcp-key.json
                        gcloud auth activate-service-account --key-file=/home/vnagavenkatesh333/key.json
                        gcloud config set project $PROJECT_ID
                        gcloud config set compute/zone us-central1-a
                    '''
                }
            }
        }

        stage('Get GKE Cluster Credentials') {
            steps {
                script {
                    // Get credentials for GKE cluster and configure kubectl
                    sh '''
                        gcloud container clusters get-credentials myprodcluster --zone us-central1-a --project globantproject-441713
                    '''
                }
            }
        }

        stage('Deploy to GKE') {
            steps {
                script {
                    // Deploy your Kubernetes resources (e.g., deployment, service, etc.)
                    sh '''
                        kubectl apply -f blue.yaml
                    '''
                }
            }
        }
    }
}
