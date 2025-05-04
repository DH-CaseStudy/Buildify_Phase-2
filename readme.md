# 📦 BuildiFy - WMS 시스템 (2차 프로젝트)

## 프로젝트개요
1차 프로젝트에서는 Java, MySQL, JDBC를 활용해 콘솔 기반의 창고관리시스템(WMS)을 개발하였습니다.
2차 프로젝트에서는 1차 프로젝트에서 구현된 내용을 Spring Framework 기반으로 확장하여 HTML, CSS, Bootstrap, JSP, MyBatis 등을 적용하여 서버사이드 렌더링(SSR) 방식의 웹 기반 시스템으로 확장하였으며, 직관적인 UI를 통해 사용자 편의성을 크게 향상시켰습니다.
더불어 Spring Security를 활용한 인증/인가 처리, 카카오 지도 API 및 주소 검색 API 연동을 통해 위치 기반 기능을 강화하였고, 데이터베이스는 AWS RDS(MySQL)로 전환하여 클라우드 환경에서의 안정성과 확장성을 확보하였습니다.
 
 물류센터의 **입·출고 요청부터 재고 현황 모니터링, 계약 관리, 보고서 생성**까지   
 창고 운영 전 과정을 웹 기반으로 자동화·시각화하는 시스템입니다.  
 
주요 목적은  
- 입·출고 처리 효율화  
- 실시간 재고 정확도 확보  
- 관리자용 대시보드를 통한 의사결정 지원  
- Excel 출력물 자동 생성
    
등을 통해 물류 운영 비용을 절감하고, 사용자 편의성을 극대화하는 것입니다.


---


## 💡 기술스택

| 영역 | 사용 기술 |
|------|-----------|
| Language | Java 17 |
| Framework | Spring , MyBatis |
| DB | MySQL |
| View | JSP |
| Cache | Spring Singleton |
| Build Tool | Gradle |
| 기타 | Lombok , Spring Security |
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
   - Gradle 설치 (wrapper 사용 시 별도 설치 불필요)  

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
	•	Spring Security 기반 로그인/로그아웃  
	•	관리자(Admin) / 사용자(User) 역할별 접근 제어  

2. 입고 관리 (Inbound)  
	•	입고 요청 등록·조회·수정  
	•	관리자 승인·반려 처리  
	•	Excel 리포트 자동 생성
	•	재고 및 입고 이력 실시간 업데이트  
   
4. 출고 관리 (Outbound)  
	•	출고 요청 등록·조회·수정·삭제  
	•	관리자 승인·반려 처리
	•	Excel 리포트 자동 생성  
	•	재고 및 출고 이력 실시간 업데이트  

6. 재고 현황  
	•	재고 현황 조회    
	•	재고 카테고리 별 조회  

7. 계약 관리  
	•	창고 임대 계약 등록·갱신  
	•	계약 기간 체크  
	•	계약별 고객 정보 관리  

8. 대시보드  
	•	관리자용 홈 화면  
	•	JavaScript 기반 차트(JSP+Chart.js)로 시각화  
	•	5분 단위 자동 리프레시  

9. 공통  
	•	글로벌 예외 처리(@ControllerAdvice) 및 커스텀 에러 페이지  
	•	Spring Singleton 캐시 활용  

10. 보안·성능  
	•	MyBatis 성능 튜닝(동적 SQL, 페이징)   
	•	트랜잭션 관리 및 롤백 보장(@Transactional)  


---

## Trouble-Shooting

![image](https://github.com/user-attachments/assets/ce766504-1f4f-4b5d-894e-063662846631)
- Spring Security 인증 객체 문제 -> 객체 생성 통해 해결
- AJAX 비동기 처리 -> AJAX기반 카테고리 조회 실패, @ResponseBody 사용해 JSON 데이터를 반환하여 해결

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

## 🏁 프로젝트요약
BuildiFy WMS는 단순한 CRUD를 넘어  
**재고 관리, 계약 관리, 대시보드 시각화까지 통합**한  
**실전형 창고 관리 시스템**입니다.

Spring MVC 구조 기반으로 **트랜잭션 통제, 비동기 처리, 성능 최적화**를 반영했으며,  
Git 협업 규칙과 코드 컨벤션을 준수하여 **팀 단위 개발 경험**을 강화했습니다.

본 프로젝트를 통해  
**"백엔드 개발자로서 구조적 설계와 협업 능력"**  
모두를 성장시킬 수 있었습니다.
