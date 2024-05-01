pipeline {
    agent { node { label 'azure-vm' } }
    stages {
        stage('build-image-result') {
            steps {
                 withCredentials([azureServicePrincipal('azureserviceprincipallatest')]) {
                 sh 'az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID' 
                 sh 'az acr login --name dvopsimages'
                 sh "cd ${WORKSPACE}/src/result; docker build -t result ."
                 sh "docker tag result dvopsimages.azurecr.io/app/result:${BUILD_NUMBER}"
               }
            }
        }
        stage('build-image-vote') {
            steps {
                 withCredentials([azureServicePrincipal('azureserviceprincipallatest')]) {
                 sh 'az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID' 
                 sh 'az acr login --name dvopsimages'
                 sh "cd ${WORKSPACE}/src/vote; docker build -t vote ."
                 sh "docker tag result dvopsimages.azurecr.io/app/vote:${BUILD_NUMBER}"
               }
            }
        }
        stage('build-image-worker') {
            steps {
                 withCredentials([azureServicePrincipal('azureserviceprincipallatest')]) {
                 sh 'az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID' 
                 sh 'az acr login --name dvopsimages'
                 sh "cd ${WORKSPACE}/src/worker; docker build -t worker ."
                 sh "docker tag result dvopsimages.azurecr.io/app/worker:${BUILD_NUMBER}"
               }
            }
        }
         stage('push-images') {
           steps {
                 withCredentials([azureServicePrincipal('azureserviceprincipallatest')]) {
                 sh 'az acr login --name dvopsimages'
                 sh "docker push dvopsimages.azurecr.io/app/result:${BUILD_NUMBER}"
                 sh "docker push dvopsimages.azurecr.io/app/vote:${BUILD_NUMBER}"
                 sh "docker push dvopsimages.azurecr.io/app/worker:${BUILD_NUMBER}"
               }
            }
            }
        }
    post { 
        always { 
            script {
                sh "docker system prune --force --all --volumes"
            }
            cleanWs()
        }
    }
    }