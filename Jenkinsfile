pipeline {
    agent any

    environment {
        IMAGE_NAME = "demo-app"
        IMAGE_TAG = "v1.0.${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/zhongym1113/demo-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Test') {
            steps {
                sh 'docker run --rm $IMAGE_NAME:$IMAGE_TAG pytest tests/'
            }
        }

        stage('Push Image') {
            steps {
                // 这里假设你已经配置好 Docker Hub 登录凭证
                def DOCKER_USERNAME = "zhong.yiming@qq.com"
                def DOCKER_PASSWORD = "uWbp@2025"
                sh '''
                echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                docker tag $IMAGE_NAME:$IMAGE_TAG $DOCKER_USERNAME/$IMAGE_NAME:$IMAGE_TAG
                docker push $DOCKER_USERNAME/$IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }

    post {
        always {
            sh 'docker image prune -f'
        }
    }
}
