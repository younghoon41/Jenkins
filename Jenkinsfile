pipeline {
    agent any

    environment {
        HARBOR_URL    = 'amdp-registry.skala-ai.com/skala26a-ai2'
        PROJECT_NAME  = 'skala26a-ai2'
        IMAGE_NAME    = 'sk052-devops-app'
        IMAGE_TAG     = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                echo '===== [1/4] GitHub 소스코드 체크아웃 ====='
                checkout scm
            }
        }
	



        stage('Code Build') {
		steps {
        		echo '===== [2/4] 코드 빌드 확인 ====='
        		sh '''
            			echo "=== 소스 파일 목록 ==="
            			ls -la
            			echo "=== requirements.txt 내용 ==="
            			cat requirements.txt
        			'''
    			}
	}

        stage('Image Build') {
            steps {
                echo '===== [3/4] Docker 이미지 빌드 ====='
                sh '''
                    docker build \
                        -t ${HARBOR_URL}/${PROJECT_NAME}/${IMAGE_NAME}:${IMAGE_TAG} \
                        -t ${HARBOR_URL}/${PROJECT_NAME}/${IMAGE_NAME}:latest \
                        .
                    echo "Image built: ${HARBOR_URL}/${PROJECT_NAME}/${IMAGE_NAME}:${IMAGE_TAG}"
                '''
            }
        }

        stage('Push to Harbor') {
            steps {
                echo '===== [4/4] Harbor Registry 에 이미지 등록 ====='
                withCredentials([usernamePassword(
                    credentialsId: 'harbor-credentials',
                    usernameVariable: 'HARBOR_USER',
                    passwordVariable: 'HARBOR_PASS'
                )]) {
                    sh '''
                        echo "Harbor 로그인 중..."
                        docker login ${HARBOR_URL} \
                            -u ${HARBOR_USER} \
                            -p ${HARBOR_PASS}

                        echo "이미지 푸시 중..."
                        docker push ${HARBOR_URL}/${PROJECT_NAME}/${IMAGE_NAME}:${IMAGE_TAG}
                        docker push ${HARBOR_URL}/${PROJECT_NAME}/${IMAGE_NAME}:latest

                        echo "Harbor 푸시 완료!"
                        docker logout ${HARBOR_URL}
                    '''
                }
            }
        }
    }

    post {
        success {
            echo """
            ✅ Pipeline 성공!
            - Image: ${HARBOR_URL}/${PROJECT_NAME}/${IMAGE_NAME}:${IMAGE_TAG}
            - Build: #${BUILD_NUMBER}
            """
        }
        failure {
            echo '❌ Pipeline 실패. 로그를 확인하세요.'
        }
        always {
            echo 'Pipeline 종료.'
        }
    }
}
