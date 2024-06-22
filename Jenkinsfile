
pipeline {
    agent any

    stages {
        stage ('Clone') {
            steps {
                git branch: 'main', url: "https://github.com/GaneshSundar96/docker-artifactory.git"
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "Artifactory",
                    url: 'http://support-team-ganeshsk-docker-nginx-test.jfrog.farm',
                    username: 'admin',
                    password: 'Sundar@96',
                )
            }
        }

        stage ('Build docker image') {
            steps {
                script {
                    docker.build(ARTIFACTORY_DOCKER_REGISTRY + '/hello-world:latest', '/resources')
                }
            }
        }

        stage ('Push image to Artifactory') {
            steps {
                rtDockerPush(
                    serverId: "Artifactory",
                    image: ARTIFACTORY_DOCKER_REGISTRY + '/hello-world:latest',
                    // Host:
                    // On OSX: "tcp://127.0.0.1:1234"
                    // On Linux can be omitted or null
                    host: HOST_NAME,
                    targetRepo: 'docker-local',
                    // Attach custom properties to the published artifacts:
                    properties: 'project-name=docker1;status=stable'
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "Artifactory"
                )
            }
        }
    }
}
