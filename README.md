
# 🌐 AWS S3 Demo Application

## 📖 프로젝트 소개
이 프로젝트는 **Spring Boot**를 사용하여 **AWS S3**와 연동한 파일 업로드 및 삭제 기능을 구현한 데모 애플리케이션입니다.  
**Postman**과 같은 API 테스트 도구를 활용하여 파일 업로드와 삭제를 간단히 수행할 수 있도록 설계되었습니다.

---

## 🏆 주요 기능
1. **파일 업로드**  
   - 사용자가 업로드한 파일을 AWS S3 버킷에 저장.
   - 업로드 성공 시, S3에 저장된 파일의 URL을 반환.

2. **이미지 업로드**  
   - 이미지 파일(`jpg`, `png`)만 업로드 가능하도록 유효성 검사 추가.
   - 업로드 후 S3에 저장된 이미지 URL을 반환.

3. **파일 삭제**  
   - S3 버킷에 저장된 파일을 이름으로 삭제.

4. **유효성 검사**  
   - 업로드된 파일의 확장자를 확인하여 유효하지 않은 파일(`exe`, `bat`) 업로드 방지.
   - 이미지 파일 업로드 시 확장자 확인 및 필터링.

---

## 📂 디렉토리 구조
```
src/
├── main/
│   ├── java/com/intheeast/
│   │   ├── configuration/      # S3 클라이언트 설정
│   │   ├── controller/         # 파일 업로드/삭제 API
│   │   ├── exception/          # 사용자 정의 예외 처리
│   │   ├── service/            # AWS S3 연동 비즈니스 로직
│   │   └── S3demoApplication   # Spring Boot 애플리케이션 엔트리
│   ├── resources/
│       ├── application.properties  # AWS 설정 (키, 리전, 버킷 정보)
```

---

## ⚙️ 주요 클래스 및 기능

### **1. S3Config**
AWS S3 클라이언트를 설정하는 클래스입니다.  
`BasicAWSCredentials`를 통해 AWS 액세스 키와 시크릿 키를 인증하며, 이를 통해 S3 버킷과의 연동을 가능하게 합니다.

```java
@Configuration
public class S3Config {
    @Value("${cloud.aws.credentials.accessKey}")
    private String accessKey;
    @Value("${cloud.aws.credentials.secretKey}")
    private String secretKey;
    @Value("${cloud.aws.region.static}")
    private String region;

    @Bean
    public AmazonS3Client amazonS3Client() {
        BasicAWSCredentials awsCredentials = new BasicAWSCredentials(accessKey, secretKey);
        return (AmazonS3Client) AmazonS3ClientBuilder.standard()
                .withRegion(region)
                .withCredentials(new AWSStaticCredentialsProvider(awsCredentials))
                .build();
    }
}
```

---

### **2. S3Controller**
API 엔드포인트를 정의한 컨트롤러 클래스입니다.

- **파일 업로드**  
  `/s3/upload`로 파일을 업로드합니다.  
  업로드 성공 시 파일의 S3 URL을 반환합니다.

- **이미지 업로드**  
  `/s3/upload-image`로 이미지를 업로드하며, 유효성 검사를 통해 이미지(`jpg`, `png`)만 허용합니다.

- **파일 삭제**  
  `/s3/delete/{fileName}`로 파일명을 전달받아 S3 버킷에서 삭제합니다.

---

### **3. S3Service**
AWS S3와의 실제 연동 로직을 처리하는 클래스입니다.

- **uploadFileToS3(MultipartFile file, String uuid)**  
  파일 메타데이터를 설정한 후 S3 버킷에 업로드합니다.

- **uploadImageToS3(MultipartFile file)**  
  이미지 파일만 업로드할 수 있도록 확장자 검사를 포함한 로직입니다.

- **deleteFile(String fileName)**  
  파일 존재 여부를 확인하고, 존재하면 삭제를 수행합니다.

---

## 🛠️ 실행 방법

1. **프로젝트 클론**
   ```bash
   git clone https://github.com/KimuJinsu/s3-demo
   cd s3-demo
   ```

2. **의존성 설치**
   ```bash
   mvn clean install
   ```

3. **애플리케이션 실행**
   ```bash
   mvn spring-boot:run
   ```

4. **Postman으로 테스트**
   - **파일 업로드**: `POST` 요청 → `http://localhost:8080/s3/upload`  
     - Body: form-data로 파일 첨부.
   - **이미지 업로드**: `POST` 요청 → `http://localhost:8080/s3/upload-image`  
     - Body: form-data로 이미지 첨부.
   - **파일 삭제**: `DELETE` 요청 → `http://localhost:8080/s3/delete/{fileName}`  
     - Path Variable: 삭제할 파일 이름.

---

## 📦 기술 스택
- **Backend**: Spring Boot, Spring MVC, Spring Data JPA
- **Cloud**: AWS S3
- **Tooling**: Maven, Postman
- **Version Control**: Git & GitHub

---

## ✨ 학습 내용
- AWS S3와 연동하여 파일 업로드/삭제 기능 구현.
- Postman을 활용한 REST API 테스트 실습.
- Spring Boot와 AWS SDK의 통합 작업 이해.
