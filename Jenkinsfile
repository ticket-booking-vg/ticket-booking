pipeline {
    agent any

    environment {
        GITHUB_ORG = 'ticket-booking-vg'
        CONTAINER_REGISTRY = "ghcr.io/${env.GITHUB_ORG}/"
    }

    stages {

        stage('Setting env variables from gradle') {
            steps {
                script {
                    // Get the artifact ID from Gradle
                    def artifactId = sh(script: './gradlew printArtifactId', returnStdout: true).trim()

                    // Set environment variables dynamically
                    env.ARTIFACT_ID = artifactId
                    env.JAR_NAME = "${env.ARTIFACT_ID}-${env.BUILD_NUMBER}"
                    env.IMAGE_NAME = "${CONTAINER_REGISTRY}${env.ARTIFACT_ID}"
                }
                sh "echo ARTIFACT_ID: ${env.ARTIFACT_ID}, JAR_NAME: ${JAR_NAME}, IMAGE_NAME: ${IMAGE_NAME}"
            }
        }

        stage('Build Application') {
            steps {
                sh "echo Performing Gradle build: ${env.ARTIFACT_ID}"
            }
        }

        stage('Build Container Image') {
            steps {
                sh "echo Building Container Image: ${env.IMAGE_NAME}"
            }
        }

        stage('Publishing Container Image') {
            steps {
                sh "echo Publishing Container Image to: ${CONTAINER_REGISTRY}"
            }
        }
    }
}
