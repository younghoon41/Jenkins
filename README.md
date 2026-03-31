# sk052 DevOps App

**작성자:** 이영훈 (A3052)  
**과제:** DevOps 이해 및 활용 1일차 - GitHub + Jenkins 기반 CI Pipeline

## 프로젝트 구조

```
.
├── app.py            # Flask 웹 애플리케이션 (소스코드)
├── requirements.txt  # Python 의존성
├── Dockerfile        # Docker 이미지 빌드 설정
├── Jenkinsfile       # Jenkins CI Pipeline 스크립트
└── README.md
```

## CI Pipeline 흐름

```
GitHub Push → Jenkins Checkout → Code Build → Image Build → Harbor Push
```

## 엔드포인트

| 경로 | 설명 |
|------|------|
| `GET /` | 기본 응답 (이름, 학번 반환) |
| `GET /health` | 헬스 체크 |

## Harbor 이미지

```
amdp-registry.skala-ai.com/skala25a/sk052-devops-app:<BUILD_NUMBER>
amdp-registry.skala-ai.com/skala25a/sk052-devops-app:latest
```
