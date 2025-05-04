# 📦 BuildiFy - WMS 시스템 (2차 프로젝트)

## 프로젝트 개요

2차 프로젝트는 1차 프로젝트(콘솔 기반 WMS)를 웹 기반 시스템으로 전환한 확장 프로젝트입니다. 주요 구현 내용은 다음과 같습니다:

- **웹 기술 적용**: HTML, CSS, Bootstrap, JSP, MyBatis, Spring Framework 기반 SSR 구현  
- **UI/UX 개선**: 사용자가 입고·출고·재고 관리를 직관적으로 할 수 있도록 웹 UI 제공  
- **보안 강화**: Spring Security를 통한 로그인 및 권한 관리 기능 적용  
- **위치 기반 서비스**: 카카오 지도 API 및 주소 검색 API 연동  
- **클라우드 환경 적용**: 로컬 DB를 AWS RDS(MySQL)로 전환하여 안정성과 확장성 확보
 
 물류센터의 **입·출고 요청부터 재고 현황 모니터링, 창고 계약 관리**까지 창고 운영 전 과정을 웹 기반으로 자동화·시각화하는 시스템입니다.  
 
주요 목적은  
- 입·출고 처리 자동화를 통한 효율성 증대  
- 실시간 재고 모니터링을 통한 효율적 운영
- 실시간 창고의 사용률, 가용률 모니터링
  
등을 통해 물류 운영 비용을 절감하고, 사용자 편의성을 극대화하는 것입니다.


---


## 💡 기술스택

| 영역 | 사용 기술 |
|------|-----------|
| Language | Java 17, HTML, CSS3 |
| Framework | Spring , MyBatis |
| DB | MySQL |
| Infra | AWS(RDS) |
| Security | Spring Security |
| View | JSP |
| Build Tool | Gradle |
| 기타 | Lombok , Log4j2 |
<br>


---

## 🚀 배포환경
- 개발환경: Local (MacOS / Windows)
- 서버: Tomcat 9.X
- DB: MySQL 8.x


---

## 📦 프로젝트구조

```
src/main/java/com.wareflow.buildify
├── cache              # 캐시
├── common             # 공통 기능 
├── config             # 설정 관련
├── constant           # 공통 상수
├── domain             # 도메인 계층 (Controller,Service,Mapper 등)
│   └── admin
│       └── inbound
│           └── controller
│           └── repository
│           └── service
│       └── ....
│   └── user
│       └── ....
├── dto                # 요청/응답 DTO
├── exception          # 예외 처리
├── mysql              
├── temp               # 임시 작업용
├── util               # 공통 유틸 클래스
└── vo                 # DB 통신 VO


src/main/resources
├── application-secret.properties   # 민감한 설정 (DB 비밀번호, 보안 키 등)
├── log4j2.xml                      # log4j2 설정 파일
├── config                          # 설정 파일
│   ├── mybatis-config.xml          # mybatis 설정 파일
├── mappers                        # Mapper 
│   ├── admin/                     # 관리자용 mapper
│   ├── users/                     # 고객용 mapper
└───└── auth/                      # 로그인용 mapper

src/main/webapp
├── static                          # 정적 파일(css, js, 이미지 등)
│   ├── css/
│   ├── fonts/
│   ├── img/
│   └── js/
├── WEB-INF
│   ├── root-context.xml
│   ├── servlet-context.xml
│   ├── web.xml
│   └── views
│   │   ├── admin/                    # 관리자 페이지
│   │   │      ├── layouts/           # header/footer/sidebar 등 관리자 레이아웃
│   │   │      ├── pages/             # 관리자 구현 페이지 모음
│   │   │      │      ├── inbound/
│   │   │      │      ├── outbound/
│   │   │      │      ├── ...                
│   │   ├── users/                    # 유저 페이지
│   │   │      ├── layouts/           # header/footer/sidebar 등 관리자 레이아웃
│   │   │      ├── pages/             # 유저 구현 페이지 모음
│   │   │      │      ├── inbound/
│   │   │      │      ├── outbound/
│   │   │      │      ├── ...           
└───└───└── common/
│   │   │      ├── pages/             
└───└───└──────└──────└── errorpage/  # 커스텀 에러페이지 구현

```
---

## ERD
![image](https://github.com/user-attachments/assets/c6ecb13e-6103-49db-9477-9505dd9f8fe9)

---

## 프로젝트 실행 가이드
 1. **환경 준비**  
   - IntelliJ, Java (17), MySQL, Gradle, Tomcat  

2. **DB 설정**  
   - `src/main/resources/application-secret.properties` 에서 DB 접속 정보 설정
     
     ```properties
     application-secret.driver=com.mysql.cj.jdbc.Driver
     application-secret.url=jdbc:mysql://localhost:3306//buildifydb?serverTimezone=Asia/Seoul
     application-secret.username=YOUR_DB_USER
     application-secret.password=YOUR_DB_PASSWORD

     ```

3. **앱 실행**  
     ```bash
     cd 프로젝트_루트_디렉터리
     ./gradlew clean build
     ./gradlew bootRun
     정상 구동 시 http://localhost:8080 에 접속 가능
    
4. **캐시/뷰 리소스 적용**  
     ```
     cache 패키지의 Singleton 빈이 정상 등록되었는지 확인
	    src/main/webapp/static 내 CSS/JS 파일 변경 시 브라우저 캐시 비우기

5. **테스트 실행**  
     ```
     bash
     ./gradlew test
---

## 🛠 주요 기능

1. 인증·인가  
   • Spring Security 기반 로그인/로그아웃  
   • 관리자(Admin) / 사용자(User) 역할별 접근 제어  

2. 입고 관리 (Inbound)  
   • 입고 요청 등록·조회·수정  
   • 관리자 승인 처리  
   • Excel 리포트 자동 생성  
   • 입고 이력 실시간 업데이트  

3. 출고 관리 (Outbound)  
   • 출고 요청 등록·조회·수정·삭제  
   • 관리자 승인·반려 처리  
   • Excel 리포트 자동 생성  
   • 재고 및 출고 이력 실시간 업데이트  

4. 재고 현황  
   • 재고 현황 조회  
   • 재고 카테고리 별 조회  

5. 계약 관리  
   • 창고 임대 계약 등록·갱신  
   • 계약 기간 체크  
   • 계약별 고객 정보 관리  

6. 대시보드  
   • 모든 사용자의 입고와 출고, 모든 지역별 창고의 사용 현황을 확인할 수 있는 관리자용 홈 화면  
   • 사용자 개인의 입고와 출고, 창고 사용 현황을 모니터링할 수 있는 유저용 홈 화면  
   • JavaScript 기반 차트(JSP + Chart.js)로 시각화  

7. 공통  
   • 공통 예외처리를 통한 커스텀 에러 페이지 리다이렉트  
   • 인증되지 않은 사용자의 특정 URL 접근 시 로그인 페이지 리다이렉트  
   • 각 도메인 별 UUID 생성 보장  

8. 보안·성능  
   • MyBatis 성능 튜닝 (동적 SQL, 페이징)  
   • 트랜잭션 관리 및 롤백 보장 (@Transactional)

---

## 🔨 내가 담당하고 구현한 기능

### 1. 📝 사용자 회원가입
- ㅇㅇㅇ
### 2. 🔐 사용자 로그인
### 3. 사용자 창고 등록
### 4. 등록된 창고 조회
### 4. 📦 사용자 상품 등록
### 6. 🔍 등록된 상품 조회
### 7. 🔍 유저 대시보드 화면

---

## Trouble-Shooting


---

## 👥 팀원
- [**김선민**](https://github.com/seonmin12)
- [**김성준**](https://github.com/kimsj18)
- [**이동휘**](https://github.com/DH-CaseStudy)
- [**신민혁**](https://github.com/minhyeokshin)

---

## 🧾 커밋, PR, 이슈 컨벤션
<br>

### ✅ 커밋 메시지 규칙

```
[이모지] 타입 : 간단한 요약

- 상세 설명 1
- 상세 설명 2 (선택)
```

| 이모지 | 타입       | 설명                         |
|--------|------------|------------------------------|
| ✨     | feature    | 새로운 기능 추가             |
| 🐛     | fix        | 버그 수정                    |
| ♻️     | refactor   | 코드 리팩토링                |
| 📝     | docs       | 문서 수정 (README 등)        |
| 💄     | style      | 코드 스타일 변경 (세미콜론, 띄어쓰기 등) |
| ✅     | test       | 테스트 코드 추가/수정        |
| 🔧     | chore      | 빌드, 설정 관련              |
| 🚀     | perf       | 성능 개선                    |
| 🔥     | remove     | 코드 삭제                    |
| 🚧     | wip        | 작업 중 (Work in progress)   |
| 🗃️     | db         | DB 관련 작업 (스키마 등)     |
| 🔀     | merge      | 브랜치 병합                  |
| 🐳     | docker     | 도커 관련 작업               |
| 🔒     | security   | 보안 관련 수정               |


---

### 📦 PR 템플릿

```md
### 🔧 작업 내용
- [ ] 작업 요약

### 📌 참고 사항
- [ ] 참고할 점
```

---

### 📌 이슈 템플릿

```md
### 📌 이슈 내용 
간단한 설명

### ✅ 작업 항목
- [ ] 할 일 1
- [ ] 할 일 2

### 💬 참고
예상되는 영향이나 고민
```

## 📌 메서드명 네이밍 규칙 (Spring Project)
- 조회: get / find / fetch
- 등록: create / save / register / add
- 수정: update / modify
- 삭제: delete / remove
- 검증: check / validate / exists
- 처리: process / handle
→ 반환되는 타입과 목적에 따라 일관성 있게 작성

📌 예시
- UserService
  - getUserById(Long id)
  - createUser(UserDTO dto)
  - updateUser(UserDTO dto)
  - deleteUser(Long id)

 ---

## 🏁 프로젝트 회고 < 정량적인 요약이 필요 > 
BuildiFy WMS는 단순한 CRUD를 넘어  
**재고 관리, 계약 관리, 대시보드 시각화까지 통합**한  
**실전형 창고 관리 시스템**입니다.

Spring MVC 구조 기반으로 **트랜잭션 통제, 비동기 처리, 성능 최적화**를 반영했으며,  
Git 협업 규칙과 코드 컨벤션을 준수하여 **팀 단위 개발 경험**을 강화했습니다.

본 프로젝트를 통해  
**"백엔드 개발자로서 구조적 설계와 협업 능력"**  
모두를 성장시킬 수 있었습니다.
