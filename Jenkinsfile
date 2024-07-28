pipeline {
    agent any

    environment {
        GITHUB_ORG = 'ticket-booking-vg'
        CONTAINER_REGISTRY = "ghcr.io/${GITHUB_ORG}/"
    }

    stages {
        stage('Setting env variables from gradle') {
            steps {
                script {
                    // Get the artifact ID from Gradle and filter the output
                    def result = sh(script: './gradlew printArtifactId', returnStdout: true).trim()
                    def artifactId = result.split('\n').find { it ==~ /^[a-zA-Z0-9\-_]+$/ }

                    // Set Groovy variables
                    def jarName = "${artifactId}-${env.BUILD_NUMBER}"
                    def imageName = "${env.CONTAINER_REGISTRY}${artifactId}"

                    // Print to check values
                    echo "ARTIFACT_ID: ${artifactId}"
                    echo "JAR_NAME: ${jarName}"
                    echo "IMAGE_NAME: ${imageName}"

                    // Set environment variables
                    env.ACTUAL_ARTIFACT_ID = artifactId
                    env.ACTUAL_JAR_NAME = jarName
                    env.ACTUAL_IMAGE_NAME = imageName
                }
            }
        }

        stage('Build Application') {
            steps {
                script {
                    sh "echo Performing Gradle build: ${env.ACTUAL_ARTIFACT_ID}"
                    sh "./gradlew -DjarName = ${env.ACTUAL_JAR_NAME} clean verify"
                }
            }
        }

        stage('Build Container Image') {
            steps {
                script {
                    sh "echo Building Container Image: ${env.ACTUAL_IMAGE_NAME}"
                }
            }
        }

        stage('Publishing Container Image') {
            steps {
                script {
                    sh "echo Publishing Container Image to: ${env.CONTAINER_REGISTRY}"
                }
            }
        }
    }
}
