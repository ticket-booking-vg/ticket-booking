pipeline {
    agent any

    environment {
        GITHUB_ORG = 'ticket-booking-vg'
        CONTAINER_REGISTRY = 'ghcr.io/${GITHUB_ORG}/'
        ARTIFACT_ID = 'TEST-ID'
        JAR_NAME = "TEST-JAR"
        IMAGE_NAME = "TEST-IMAGE"
    }

    stages {

        stage('Setting env variables from gradle') {
                    steps {
                        def result = sh(script: './gradlew printArtifactId', returnStdout: true).trim()

                                            // Set the artifact ID as an environment variable
                        environment.ARTIFACT_ID = result
                        environment.JAR_NAME = '${ARTIFACT_ID}-${BUILD_NUMBER}'
                        environment.IMAGE_NAME = '${CONTAINER_REGISTRY}${ARTIFACT_ID}'

                    }
                }

        stage('Build Application') {
            steps {
                sh 'echo Performing Gradle build: ${ARTIFACT_ID}'
            }
        }

        stage('Build Container Image') {
                    steps {
                        sh 'echo Building Container Image: ${IMAGE_NAME}'
                    }
                }

        stage('Publishing Container Image') {
                    steps {
                        sh 'echo Publishing Container Image to: ${CONTAINER_REGISTRY}'
                    }
                }

    }
}