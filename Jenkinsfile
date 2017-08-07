#!/usr/bin/env groovy

node {
  identifier = "Swagger UI:$BRANCH_NAME - Build ID: $BUILD_NUMBER"
  checkout scm
  stage ('Docker build') {
    slackSend color: "warning", message: "Starting image build for: ${identifier}"
    try {
      image = docker.build('swagger-ui:$BRANCH_NAME')
      slackSend color: "good", message: "Image build succeeded for: ${identifier}"
    }
    catch (exc) {
      slackSend color: "danger", message: "Image build failed for: ${identifier}"
    }
  }
  stage ('Docker push') {
    slackSend color: "warning", message: "Pushing image to AWS repository: ${identifier}"
    try {
      docker.withRegistry('https://024673053271.dkr.ecr.us-west-2.amazonaws.com', 'ecr:us-west-2:aws-test') {
        image.push('${BRANCH_NAME}_latest')
      }
      slackSend color: "good", message: "Push complete for: ${identifier}"
    } catch (exc) {
      slackSend color: "danger", message: "Push failed for: ${identifier}"
    }
  }
}
