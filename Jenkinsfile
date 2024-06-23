pipeline {
2    agent any
3    environment {
4        // Variables
5        DOCKER_IMAGE = ‘busy box’
6        IMAGE_TAG = 'latest'
7        ARTIFACTORY_URL = 'http://support-team-ganeshsk-docker-nginx-test.jfrog.farm’
8        ARTIFACTORY_DOCKER_REPO = 'docker-local.support-team-ganeshsk-docker-nginx-test.jfrog.farm'
9        REGISTRY_CREDENTIALS_ID = 'Jfrog'
10    }
11    stages {
12        stage('Build Image') {
13            steps {
14                script {
15                    // Building the Docker image
16                    def customImage = docker.build("${DOCKER_IMAGE}:${IMAGE_TAG}")
17                }
18            }
19        }
20        stage('Push to Artifactory') {
21            steps {
22                script {
23                    docker.withRegistry("${ARTIFACTORY_URL}/${ARTIFACTORY_DOCKER_REPO}", "${REGISTRY_CREDENTIALS_ID}") {
24                        // Pushing the image to Artifactory
25                        customImage.push("${IMAGE_TAG}")
26                    }
27                }
28            }
29        }
30    }
31    post {
32        success {
33            echo 'Process completed successfully.'
34        }
35        failure {
36            echo 'Something went wrong.'
37        }
38    }
39}

