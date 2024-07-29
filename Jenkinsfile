pipeline {
    agent { label "ecsAgent"}

    environment {
        AWS_CREDENTIALS_ID = 'AKIAVRUVWNDVTKL4Q2U7'
        ECR_REPOSITORY = 'public.ecr.aws/b3y8b3n1/jenkinsecs'
        ECS_CLUSTER = 'ClusterForJenkins'
        ECS_SERVICE = 'JenkinsAuthApp'
        IMAGE_TAG = "${env.BUILD_ID}"
    }

    stages {
        stage('Build') {
            steps {
                script {
                    docker.build("${env.ECR_REPOSITORY}:${env.IMAGE_TAG}")
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    docker.withRegistry('https://381492291819.dkr.ecr.ca-central-1.amazonaws.com', 'ecr:ca-central-1:aws-credentials') {
                        docker.image("${env.ECR_REPOSITORY}:${env.IMAGE_TAG}").push()
                    }
                }
            }
        }

        stage('Deploy to ECS') {
            steps {
                script {
                    def taskDefinition = sh(script: """
                        aws ecs describe-services --cluster ${env.ECS_CLUSTER} --services ${env.ECS_SERVICE} | jq -r '.services[0].taskDefinition'
                    """, returnStdout: true).trim()

                    def newTaskDefinition = sh(script: """
                        aws ecs describe-task-definition --task-definition ${taskDefinition} | jq '.taskDefinition | del(.status, .revision, .taskDefinitionArn, .requiresAttributes, .compatibilities)' | jq '.containerDefinitions[0].image = "${env.ECR_REPOSITORY}:${env.IMAGE_TAG}"'
                    """, returnStdout: true).trim()

                    sh """
                        echo '${newTaskDefinition}' > new-task-def.json
                        aws ecs register-task-definition --cli-input-json file://new-task-def.json
                        aws ecs update-service --cluster ${env.ECS_CLUSTER} --service ${env.ECS_SERVICE} --task-definition ${taskDefinition}
                    """
                }
            }
        }
    }
}
