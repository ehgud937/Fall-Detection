# 실시간 낙상감지 시스템 (Real-time Fall Detection System)

이 프로젝트는 컴퓨터 비전 기술을 활용하여 실시간으로 낙상 상황을 감지하고 알림을 제공하는 시스템입니다. YOLOv8 포즈 추정 모델을 기반으로 사람의 자세를 분석하여 낙상 여부를 판단합니다.

## 주요 기능

- **실시간 낙상 감지**: 카메라 영상에서 실시간으로 낙상 상황을 감지
- **포즈 분석**: 신체 키포인트 추적을 통한 자세 분석 및 평가
- **모션 컨텍스트 분석**: 시간에 따른, 움직임, 속도, 가속도를 추적하여 정확한 낙상 감지
- **자동 알림 시스템**: 낙상 감지 시 이미지 캡처 및 알림 발송
- **직관적 시각화**: 스켈레톤 모델 및 디버그 정보 실시간 표시

## 동작 영상
![output_1](https://github.com/user-attachments/assets/7b175c4c-a555-437c-b70c-4599425d41df)
![output_2](https://github.com/user-attachments/assets/e9c610af-9645-418d-b2b5-1b0a1cbda16b)
![output_3](https://github.com/user-attachments/assets/8c883082-f862-4a24-89b9-409612c16744)
![자료1](https://github.com/user-attachments/assets/2ed5befd-9527-48e7-8831-a5a8379f260e)
![자료2](https://github.com/user-attachments/assets/23dc8577-f034-4d99-893a-3b93f4610431)

### 핵심 컴포넌트

1. **Config**: 모델 경로, 임계값, 시각화 설정 등 구성 관리
2. **MotionContext**: 사람의 움직임 내역 및 활동 패턴 추적
3. **EnhancedPoseAnalyzer**: 포즈 데이터 분석 및 낙상 판단 알고리즘
4. **FPSTracker**: 실시간 프레임 처리 성능 모니터링
5. **EnhancedFallDetector**: 전체 감지 시스템 통합 및 관리

### 낙상 감지 알고리즘

낙상 감지는 다음 4가지 핵심 요소를 종합적으로 분석하여 이루어집니다:

1. **수직 정렬 분석 (30%)**: 신체의 수직 정렬 상태 평가
2. **자세 분석 (20%)**: 상체와 하체의 자세 및 기울기 분석
3. **관절 각도 분석 (20%)**: 무릎 등 주요 관절의 각도 분석
4. **모션 컨텍스트 분석 (30%)**: 속도, 가속도, 안정성 등 모션 패턴 분석

낙상 점수가 0.7을 초과하고 해당 상태가 2초 이상 지속되면 낙상으로 판단합니다.

## 설치 방법

### 요구사항

- Python 3.8+
- PyTorch 2.0+
- OpenCV 4.5+
- Ultralytics YOLOv8
- numpy
- aiohttp

### 설치 과정

```bash
# 저장소 클론
git clone https://github.com/yourusername/fall-detection-system.git
cd fall-detection-system

# 의존성 설치
pip install -r requirements.txt

# YOLOv8 포즈 모델 다운로드 (자동으로 다운로드되지 않는 경우)
# 모델은 첫 실행 시 자동으로 다운로드됩니다
```

## 사용 방법

### 기본 실행

```bash
python fall_detection.py
```

### 설정 변경

`Config` 클래스에서 다음 값들을 조정할 수 있습니다:

- `model_path`: 사용할 YOLOv8 모델 경로
- `confidence_threshold`: 감지 신뢰도 임계값 (기본값: 0.4)
- `fall_duration_threshold`: 낙상으로 판단할 최소 지속 시간 (기본값: 2.0초)
- `api_url`: 알림을 보낼 API 엔드포인트

## 작동 원리

### 데이터 흐름

1. **비디오 입력**: 웹캠이나 비디오 파일에서 프레임 획득
2. **포즈 추정**: YOLOv8 모델로 키포인트 추출
3. **모션 분석**: 최근 프레임의 움직임 패턴 분석
4. **포즈 분석**: 수직 정렬, 자세, 관절 각도 등 분석
5. **낙상 평가**: 종합적인 분석 결과로 낙상 여부 판단
6. **알림 처리**: 낙상 감지 시 이미지 캡처 및 알림 발송

### 고급 분석 기능

1. **활동 분류**:
   - 정지 상태 (stationary)
   - 느린 움직임 (slow_movement)
   - 걷기 (walking)
   - 빠른 움직임 (rapid_movement)

2. **자세 안정성 평가**: 최근 자세의 안정성을 수치화하여 분석

3. **관절 각도 분석**: 무릎 각도 등을 분석하여 낙상 패턴 감지

## 시각화 정보

실시간 화면에 표시되는 정보는 다음과 같습니다:

- **스켈레톤 표시**: 초록색(정상), 빨간색(낙상 감지)
- **키포인트**: 노란색 원으로 표시
- **낙상 상태**: 화면 상단에 "FALL DETECTED" 표시
- **디버그 정보**: 좌측에 낙상 점수 및 세부 분석 결과 표시
- **FPS 정보**: 좌측 상단에 현재 처리 속도 표시

## 알림 시스템

낙상이 감지되면:

1. 현재 프레임을 이미지로 캡처
2. 지정된 API 서버로 이미지 업로드
3. 알림 API를 통해 가족 구성원에게 알림 전송

## 문제 해결

- **모델 로드 실패**: YOLOv8 모델 파일이 올바른 경로에 있는지 확인
- **카메라 액세스 오류**: 카메라 장치 번호가 올바른지 확인
- **낮은 FPS**: 모델 크기를 조정하거나 디스플레이 해상도를 낮춤
- **오탐지**: `confidence_threshold` 값을 높이거나 `fall_duration_threshold` 값을 늘림

## 기여 방법

1. 이 저장소를 포크합니다
2. 새 기능 브랜치를 생성합니다 (`git checkout -b feature/amazing-feature`)
3. 변경사항을 커밋합니다 (`git commit -m 'Add some amazing feature'`)
4. 브랜치에 푸시합니다 (`git push origin feature/amazing-feature`)
5. Pull Request를 생성합니다

## 라이센스

이 프로젝트는 MIT 라이센스 하에 배포됩니다. 자세한 내용은 `LICENSE` 파일을 참고하세요.

## 감사의 말

- YOLOv8 개발 팀
- PyTorch 커뮤니티
- 테스트 및 피드백을 제공한 모든 사용자
