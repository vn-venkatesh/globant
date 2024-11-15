pipeline {
    agent any

    environment {
        // Define environment variables
        PROJECT_ID = 'globantproject-441713'                       // Your GCP Project ID
        CLUSTER_NAME = 'myprodcluster'                       // Your GKE Cluster Name
        ZONE = 'us-central1-a'                               // GKE Cluster Zone
        DEPLOYMENT_FILE = 'k8s/deployment.yaml'                  // Path to your Kubernetes manifest
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clones  the repository from the specified Git URL
                git "https://github.com/vn-venkatesh/my_mainfestfiles"
            }
        }
        stage('Authenticate with Google Cloud') {
            steps {
                script {
                    // Authenticate using the service account key
                    sh '''
                        export PATH=$PATH:/usr/local/google-cloud-sdk/bin
                        gcloud auth activate-service-account --key-file=/usr/local/key.json
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
                        export PATH=$PATH:/usr/local/google-cloud-sdk/bin
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
                        export PATH=$PATH:/usr/local/google-cloud-sdk/bin
                        kubectl apply -f my_mainfestfiles/blue.yaml
                    '''
                }
            }
        }
    }
}
