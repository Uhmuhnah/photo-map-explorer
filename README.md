# photo-map-explorer
Photo Map Explorer는 사진 속에 숨어 있는 공간적·시간적 맥락을 되살리는 웹 기반 탐색 도구입니다.  
EXIF 메타데이터(특히 GPS 좌표와 촬영 시각)를 활용하여 사용자의 사진을 지도와 타임라인 위에서 재구성함으로써, 평면적 갤러리에서 보여주지 못한 경험을 제공합니다.

---

## ✨ Features
- **구글 드라이브 파일 연결**
  구글 드라이브 속 파일 안에 있는 이미지를 브라우저로 가져옵니다. (이 프로젝트는 GPS를 포함한 EXIF 파일이 존재하는 사진이 담긴 파일이 필요합니다. 데모 버전 실행할 시 PNG 데이터가 저장된 공유 드라이브 파일을 사용하셔도 됩니다. [예제 파일 링크](https://drive.google.com/drive/folders/1e88VHekL-d0GUsKvBJNYKExsuyJupqCS?usp=drive_link)
- **지도 기반 사진 탐색**  
  EXIF GPS 정보를 기반으로 사진을 Mapbox 지도 위에 시각적으로 배치합니다.

- **타임라인 탐색**  
  촬영된 시점에 따라 시간의 흐름을 따라 사진을 감상할 수 있습니다.

- **basic Mode**  
  위의 지도와 타임라인을 이용하여 사진을 색다르게 감상할 수 있습니다.
  
- **Glow Mode**  
  사진에서 주요 색상을 추출하여 지도에 컬러를 입혀 도시 야경과 불꽃놀이같이 새로운 느낌으로 사진이 담은 추억을 돌이켜 볼 수 있게 했습니다.

- **Density Mode**  
  시도/시군구별 사진 개수를 계산하여 데이터 기반의 공간 분포를 직관적으로 보여줍니다. -> "아 내가 8월달에는 이 동네에서 시간을 많이 보냈구나"와 같이 지난 시간을 되돌아 볼 수 있게 의도했습니다.

---

## 📸 Screenshots / (편의상 Darkmode로 진행) 
**전체 UI** 
<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/8630f82d-fcc5-443a-a5a6-d2637f183e3b" />
구글 드라이브 연결 후 파일 선택에서 원하는 사진이 담긴 파일을 선택하고 사진 불러오기하면 됩니다.

**Basic 모드**
<img width="1390" height="907" alt="basic3" src="https://github.com/user-attachments/assets/2e5a2c66-6b79-4ace-b90f-e5622ca66554" />
**특정 기간 모습**
<img width="1391" height="791" alt="basic2" src="https://github.com/user-attachments/assets/e519667d-c709-4240-abbe-432ce9f92842" />
**Glow 모드**
<img width="1918" height="907" alt="glow3" src="https://github.com/user-attachments/assets/f43fcbed-fbdd-401f-bc67-7d10090e86b6" />
<img width="1917" height="912" alt="glow2" src="https://github.com/user-attachments/assets/440e024e-af3b-4860-8471-ab233391f288" />
**Glow 모드**
<img width="1917" height="908" alt="glow1" src="https://github.com/user-attachments/assets/8364c896-dbc1-42aa-8fa1-e1fbad3b01b0" />
**Density 모드**
<img width="1918" height="905" alt="density2" src="https://github.com/user-attachments/assets/095c2034-a3e6-4962-b13b-c98ca6da7de1" />
**Density 모드**
<img width="1917" height="911" alt="density1" src="https://github.com/user-attachments/assets/ef99e51c-66ba-4e9b-a757-53895c4db43f" />

---
## Tech Stack

- Frontend: HTML, CSS, JavaScript

- Mapping: Mapbox GL JS

- Metadata: EXIF GPS, Timestamp
---


🗄 Data Model Overview

본 프로젝트는 사진 메타데이터 기반의 분석과 시각화를 위해 다음과 같은 정보를 처리합니다.

### 사진에서 추출하는 EXIF 데이터 
GPS 좌표 (latitude, longitude)

촬영 시각 (timestamp)

### 행정구역 단위 (sido, sigungu)
행정구역 단위 json 파일의 출처는 다음과 같습니다.
[시도/ 시군구 행정구역 json 파일 출처](https://jgws.tistory.com/entry/%EB%8C%80%ED%95%9C%EB%AF%BC%EA%B5%AD-%EB%B2%95%EC%A0%95%EA%B5%AC%EC%97%AD-SHP-%ED%8C%8C%EC%9D%BC%EC%9D%84-GeoJSON%EC%9C%BC%EB%A1%9C-%EB%B3%80%ED%99%98%ED%95%98%EA%B8%B0)

날씨 정보 (weather_id 기반 참조)

## 사용 중인 외부 서비스 및 API

이 프로젝트 **Photo Map Explorer**는 다음 세 가지 외부 서비스/API를 사용합니다.

### 1. Mapbox (지도 시각화)

- **역할**:  
  - 전 세계 지도를 불러와서 사진 위치를 시각화
  - 기본 모드, Glow 모드, Density(촬영 밀도) 모드 등 지도 스타일/레이어 구성에 사용
- **사용 기술**
  - [`mapbox-gl-js`](https://docs.mapbox.com/mapbox-gl-js/)  
  - 스타일 예시: `mapbox://styles/mapbox/dark-v11`, `mapbox://styles/mapbox/light-v11`
- **인증 방식**
  - Mapbox Access Token을 사용 (클라이언트 사이드에서 `mapboxgl.accessToken` 설정)
 
    
### 2. Google Drive API (사진 메타데이터 및 이미지)
- **역할**:
  - 사용자가 지정한 Google Drive 폴더에서 이미지 파일 목록을 가져옴
  - 각 이미지의 EXIF 메타데이터(위도/경도, 촬영 시간, 썸네일 등)를 읽어 지도에 표시
- **사용 API**
  - Google OAuth 2.0 (Client-side)
  - Google Drive API v3


읽기 전용으로만 사용하며, 사용자의 Drive 파일을 수정하지 않음

### 3. Open-Meteo Archive API (과거 일별 날씨 데이터)
- **역할**:
  - 각 사진이 찍힌 날짜·위치(위도/경도)에 대한 일 평균 기온 / 강수량 / 날씨 코드를 조회
  - 결과를 기반으로 사진 주변에 “날씨 링”을 그려 시각화
- **사용 API**
  - Open-Meteo Archive API
- **요청 파라미터 예시**
  - latitude, longitude : 사진의 위도/경도
  - start_date, end_date : YYYY-MM-DD (단일 날짜 기준)
  - daily : temperature_2m_mean,precipitation_sum,weathercode
  - timezone : Asia/Seoul



