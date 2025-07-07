# 🥛 우유급식 희망서 시스템

초등학교 우유급식 신청을 위한 디지털 가정통신문 시스템입니다.

[![GitHub License](https://img.shields.io/github/license/yourusername/milk-application-system)](LICENSE)
[![GitHub Stars](https://img.shields.io/github/stars/yourusername/milk-application-system)](https://github.com/yourusername/milk-application-system/stargazers)
[![GitHub Issues](https://img.shields.io/github/issues/yourusername/milk-application-system)](https://github.com/yourusername/milk-application-system/issues)

## 🌟 주요 기능

### 📝 학부모용 신청서 (`index.html`)
- **반응형 웹 디자인**: 모바일, 태블릿, PC 모든 기기 지원
- **디지털 서명**: 마우스 그리기 또는 키보드 입력 선택 가능
- **실시간 미리보기**: 서명 미리보기 기능
- **유효성 검증**: 필수 항목 입력 확인
- **직관적인 UI**: 이모지와 그라데이션을 활용한 친화적 디자인

### 📊 교사용 관리 시스템 (`teacher.html`)
- **실시간 대시보드**: 신청 현황 통계
- **고급 필터링**: 학년, 반, 희망여부별 검색
- **서명 확대보기**: 모달을 통한 서명 상세 확인
- **데이터 내보내기**: CSV 형식으로 다운로드
- **구글 시트 연동**: 직접 링크를 통한 원본 데이터 접근
- **학급별 통계**: 시각적 통계 정보 제공

## 🚀 시작하기

### 1단계: 구글 시트 준비

1. **구글 시트 생성**
   ```
   https://sheets.google.com → 새 스프레드시트 만들기
   ```

2. **헤더 설정** (첫 번째 행에 다음 항목들 입력)
   ```
   제출시간 | 학년 | 반 | 번호 | 학생명 | 학부모명 | 서명 | 서명타입 | 우유급식희망
   ```

3. **스프레드시트 ID 복사**
   ```
   URL: https://docs.google.com/spreadsheets/d/[SPREADSHEET_ID]/edit
   ```

### 2단계: Google Apps Script 설정

1. **새 프로젝트 생성**
   ```
   https://script.google.com → 새 프로젝트
   ```

2. **코드 붙여넣기**
   ```javascript
   function doPost(e) {
     try {
       // 여기에 스프레드시트 ID 입력
       const SPREADSHEET_ID = 'YOUR_SPREADSHEET_ID_HERE';
       const sheet = SpreadsheetApp.openById(SPREADSHEET_ID).getActiveSheet();
       
       const data = JSON.parse(e.postData.contents);
       
       const newRow = [
         data.timestamp,
         data.grade + '학년',
         data.class + '반',
         data.number + '번',
         data.studentName,
         data.parentName,
         data.signature,
         data.signatureType,
         data.milkPreference
       ];
       
       sheet.appendRow(newRow);
       
       return ContentService
         .createTextOutput(JSON.stringify({success: true, message: '데이터가 성공적으로 저장되었습니다.'}))
         .setMimeType(ContentService.MimeType.JSON);
         
     } catch (error) {
       return ContentService
         .createTextOutput(JSON.stringify({success: false, message: '오류가 발생했습니다: ' + error.toString()}))
         .setMimeType(ContentService.MimeType.JSON);
     }
   }

   function doGet(e) {
     try {
       const SPREADSHEET_ID = 'YOUR_SPREADSHEET_ID_HERE';
       const sheet = SpreadsheetApp.openById(SPREADSHEET_ID).getActiveSheet();
       
       const range = sheet.getDataRange();
       const values = range.getValues();
       
       if (values.length <= 1) {
         return ContentService
           .createTextOutput(JSON.stringify({success: true, data: []}))
           .setMimeType(ContentService.MimeType.JSON);
       }
       
       const headers = values[0];
       const data = values.slice(1).map(row => {
         const obj = {};
         headers.forEach((header, index) => {
           obj[header] = row[index];
         });
         return obj;
       });
       
       return ContentService
         .createTextOutput(JSON.stringify({success: true, data: data}))
         .setMimeType(ContentService.MimeType.JSON);
         
     } catch (error) {
       return ContentService
         .createTextOutput(JSON.stringify({success: false, message: '오류가 발생했습니다: ' + error.toString()}))
         .setMimeType(ContentService.MimeType.JSON);
     }
   }
   ```

3. **웹 앱으로 배포**
   - 배포 → 새 배포
   - 유형: 웹 앱
   - 실행 대상: 나
   - 액세스 권한: **모든 사람**
   - 배포 → 웹 앱 URL 복사

### 3단계: HTML 파일 설정

1. **`index.html` 설정**
   ```javascript
   // 43번째 줄 근처
   const GOOGLE_SCRIPT_URL = '여기에_복사한_웹앱_URL_붙여넣기';
   ```

2. **`teacher.html` 설정**
   ```javascript
   // 웹 앱 URL 설정
   const GOOGLE_SCRIPT_URL = '여기에_복사한_웹앱_URL_붙여넣기';
   
   // 구글 시트 URL 설정 (선택사항)
   const GOOGLE_SHEETS_URL = '여기에_구글시트_URL_붙여넣기';
   ```

## 📁 프로젝트 구조

```
milk-application-system/
│
├── index.html          # 학부모용 신청서
├── teacher.html        # 교사용 관리 시스템
├── README.md          # 프로젝트 문서
└── gas-code.js        # Google Apps Script 코드 (참고용)
```

## 🛠️ 기술 스택

- **Frontend**: HTML5, CSS3, JavaScript (ES6+)
- **Backend**: Google Apps Script
- **Database**: Google Sheets
- **Styling**: CSS Grid, Flexbox, 그라데이션
- **Icons**: 이모지
- **Responsive**: CSS Media Queries

## 🎨 UI/UX 특징

- **모던 디자인**: 그라데이션과 그림자 효과
- **직관적 아이콘**: 이모지를 활용한 사용자 친화적 인터페이스
- **반응형**: 모든 기기에서 최적화된 경험
- **접근성**: 고대비 색상과 의미론적 마크업
- **애니메이션**: 부드러운 호버 효과와 전환

## 📱 지원 기기

- **데스크톱**: Chrome, Firefox, Safari, Edge
- **모바일**: iOS Safari, Android Chrome
- **태블릿**: iPad, Android 태블릿

## 🔧 문제 해결

### CORS 오류 발생 시
```javascript
// fetch 요청에서 Content-Type 변경
headers: {
  'Content-Type': 'text/plain;charset=utf-8'  // application/json 대신 사용
}
```

### 데이터가 저장되지 않을 때
1. Google Apps Script의 스프레드시트 ID 확인
2. 웹 앱 배포 시 "액세스 권한: 모든 사람" 설정 확인
3. 구글 시트 헤더가 정확히 설정되어 있는지 확인

### 교사용 페이지에 데이터가 안 보일 때
1. `doGet` 함수의 스프레드시트 ID 설정 확인
2. 브라우저 개발자 도구 → Console 에서 오류 메시지 확인

## 🔐 보안 고려사항

- **개인정보 보호**: 서명 데이터는 base64로 인코딩되어 저장
- **접근 제한**: 구글 시트 공유 설정으로 접근 권한 관리
- **데이
