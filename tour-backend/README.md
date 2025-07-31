# TourProject Backend

## 📋 프로젝트 개요
여행 계획 관리 시스템의 백엔드 API 서버입니다. Spring Boot를 기반으로 구축되었으며, 여행 계획의 생성, 수정, 조회, 삭제 기능을 제공합니다.

## 🏗️ 아키텍처

### 주요 구성 요소
- **Controller Layer**: REST API 엔드포인트 제공
- **Service Layer**: 비즈니스 로직 처리
- **Repository Layer**: 데이터 액세스 
- **Domain Layer**: 엔티티 및 도메인 객체
- **Utility Layer**: 공통 유틸리티 및 헬퍼 클래스

## 🔧 최근 업데이트 (2025.07.10)

### ✅ 완료된 작업들

#### 1. **TourDefaults 클래스 구현**
- 여행 계획 관련 기본값 및 정책 통합 관리
- 기본값 적용, 유효성 검증, 메타데이터 관리 기능
- 중복 로직 제거 및 코드 재사용성 향상

#### 2. **JsonUtil 강화**
- 안전한 JSON 변환 메소드 추가 (`toJsonSafe`, `fromJsonSafe`)
- JSON 유효성 검사 기능
- 예외 발생 시 기본값 반환 로직
- 알 수 없는 속성 무시 설정

#### 3. **ValidationUtil 신규 생성**
- 입력 데이터 유효성 검증 통합 관리
- TourDto 전체 검증 기능
- 개별 필드별 검증 메소드
- 보안을 고려한 문자열 정리 기능

#### 4. **ResponseUtil 신규 생성**
- API 응답 표준화
- 성공/실패 응답 통일
- 다양한 HTTP 상태 코드별 헬퍼 메소드
- 페이징 및 조건부 응답 지원

#### 5. **GlobalExceptionHandler 확장**
- Tour 관련 커스텀 예외 추가
- 구조화된 에러 응답
- 로깅 시스템 통합
- 사용자 친화적 에러 메시지

#### 6. **TourService 개선**
- ValidationUtil과 TourDefaults 통합 적용
- 커스텀 예외 사용으로 에러 처리 개선
- 코드 중복 제거 및 가독성 향상
- deprecated 메소드 정리

#### 7. **TourController 리팩토링**
- ResponseUtil 적용으로 응답 표준화
- 예외 처리 로직 간소화
- 새로운 API 엔드포인트 추가 (복사, 개수 조회)
- CORS 설정 유지

#### 8. **SampleDataLoader 업데이트**
- TourDefaults 사용으로 기본값 정책 통일
- 중복 계산 로직 제거

## 🛠️ 기술 스택
- **Java 17**
- **Spring Boot 3.x**
- **Spring Data JPA**
- **Spring Security**
- **H2 Database** (개발용)
- **Lombok**
- **Jackson** (JSON 처리)
- **Gradle** (빌드 도구)

## 📁 프로젝트 구조

```
src/main/java/com/example/tour_backend/
├── config/                 # 설정 클래스들
│   ├── JwtTokenProvider     # JWT 토큰 처리
│   ├── SampleDataLoader     # 샘플 데이터 로더
│   └── SecurityConfig       # 보안 설정
├── controller/              # REST 컨트롤러
│   ├── TourController       # 여행 계획 API
│   ├── UserController       # 사용자 API
│   └── ...
├── domain/                  # 도메인 엔티티
│   ├── tour/               # 여행 관련 엔티티
│   ├── user/               # 사용자 엔티티
│   └── ...
├── dto/                     # 데이터 전송 객체
│   ├── tour/               # 여행 관련 DTO
│   ├── common/             # 공통 DTO (ApiResponse 등)
│   └── ...
├── exception/               # 예외 처리
│   └── GlobalExceptionHandler
├── service/                 # 비즈니스 로직
│   ├── TourService         # 여행 계획 서비스
│   └── ...
└── util/                    # 유틸리티 클래스
    ├── JsonUtil            # JSON 처리 유틸
    ├── ValidationUtil      # 유효성 검증 유틸
    ├── ResponseUtil        # API 응답 유틸
    └── tour/
        └── TourDefaults    # 여행 기본값 관리
```

## 🚀 주요 API 엔드포인트

### 여행 계획 관리
- `POST /api/tours` - 새로운 여행 계획 생성
- `GET /api/tours/{tourId}` - 여행 계획 조회
- `PUT /api/tours/{tourId}` - 여행 계획 수정
- `DELETE /api/tours/{tourId}` - 여행 계획 삭제
- `GET /api/tours/user/{userId}` - 사용자별 여행 계획 목록
- `POST /api/tours/{tourId}/copy` - 여행 계획 복사
- `GET /api/tours/user/{userId}/count` - 사용자 여행 계획 개수

## 📊 API 응답 형식

### 성공 응답
```json
{
  "success": true,
  "message": "요청이 성공적으로 처리되었습니다.",
  "data": { /* 실제 데이터 */ },
  "timestamp": "2025-07-10T10:30:00"
}
```

### 에러 응답
```json
{
  "success": false,
  "message": "에러 메시지",
  "data": {
    "errorCode": "ERROR_CODE",
    "details": "상세한 오류 정보는 로그를 확인하세요."
  },
  "timestamp": "2025-07-10T10:30:00"
}
```

## 🔧 개발 환경 설정

### 1. 프로젝트 클론
```bash
git clone [repository-url]
cd TourProject/tour-backend
```

### 2. 빌드 및 실행
```bash
# Windows
gradlew.bat clean build
gradlew.bat bootRun

# Linux/Mac
./gradlew clean build
./gradlew bootRun
```

### 3. 개발 프로필 실행
```bash
# 샘플 데이터와 함께 실행
gradlew.bat bootRun --args='--spring.profiles.active=dev'
```

## 📋 유효성 검증 규칙

### 여행 계획 생성/수정 시
- **제목**: 필수, 최대 100자, HTML 태그 불허
- **여행자 수**: 1-50명 범위
- **여행 기간**: 오늘 이후 날짜, 최대 365일
- **시작일/종료일**: 필수, 시작일 ≤ 종료일

## 🔒 보안 정책

### 입력 데이터 정리
- HTML 태그 자동 제거
- Script 관련 문자열 필터링
- XSS 방지를 위한 문자열 검증

### 예외 처리
- 사용자 입력 오류: 400 Bad Request
- 리소스 없음: 404 Not Found
- 서버 내부 오류: 500 Internal Server Error

## 📝 로깅 정책

### 로그 레벨별 기록 내용
- **INFO**: API 요청/응답, 주요 비즈니스 로직 수행
- **WARN**: 유효성 검증 실패, 예상 가능한 오류
- **ERROR**: 예상하지 못한 예외, 시스템 오류

## 🧪 테스트

```bash
# 전체 테스트 실행
gradlew.bat test

# 컴파일만 확인
gradlew.bat compileJava
```

## 🚀 배포 준비사항

### 프로덕션 환경 체크리스트
- [ ] 데이터베이스 설정 변경 (H2 → MySQL/PostgreSQL)
- [ ] 보안 설정 강화
- [ ] 로깅 설정 최적화
- [ ] 성능 모니터링 설정
- [ ] CORS 설정 검토

## 📞 문의 및 지원

개발 관련 문의나 버그 리포트는 이슈 트래커를 통해 등록해 주세요.

---
**마지막 업데이트**: 2025년 7월 10일  
**버전**: 1.2.0  
**개발자**: 풀스택 개발팀
