# Image Processing GUI Program

OpenCV와 OpenGL(GLUI)을 이용하여 구현한 영상 처리 GUI 프로그램입니다.  
학부 영상처리 과제 제출용으로 작성되었으며, 이미지 로드/저장, 드로잉, 크롭, 명암 조절, 이진화 기능을 제공합니다.

## Overview

이 프로그램은 마우스와 GLUI 기반 메뉴를 이용하여 이미지에 직접 도형을 그리거나,  
영상의 특정 영역을 선택하여 잘라내고, 다양한 영상 처리 기능을 적용할 수 있도록 구현되었습니다.

주요 목표는 다음과 같습니다.

- 컬러/그레이스케일 영상 불러오기
- 처리 결과 저장
- 마우스 기반 드로잉 및 크롭
- Contrast 조절
- Thresholding 적용

---

## Features

### 1. Image I/O
- `READ` 버튼으로 이미지 파일 불러오기
- `Save` 버튼으로 현재 결과 이미지 저장
- 저장 시 확장자가 없으면 `.png`로 저장

### 2. Drawing
GLUI 메뉴를 통해 다음 도형을 선택하여 이미지 위에 그릴 수 있습니다.

- **PEN**: 자유 곡선 드로잉
- **LINE**: 시작점과 끝점을 지정하여 직선 그리기
- **RECTANGLE**: 직사각형 그리기
- **CIRCLE**: 원 그리기

추가 옵션:
- **THICK**: 선 두께 조절
- **RED / GREEN / BLUE**: 색상 조절
- **Fill**: 원 채우기 지원  
  - `CIRCLE`에서 체크 시 내부 채움 적용
  - `RECTANGLE`은 코드상 채움이 직접 반영되지는 않음

### 3. Cropping
- `Cropping` 버튼 클릭 후 마우스로 영역 선택
- 드래그한 사각형 영역을 잘라내어 현재 이미지로 갱신
- `ESC` 키를 누르면 크롭 모드 종료

### 4. Contrast Adjustment
다음 명암 조절 기능을 제공합니다.

- **Linear Contrast**
  - `(X1, Y1)`, `(X2, Y2)`를 기준으로 piecewise linear contrast transform 수행
- **Inverse**
  - 픽셀 값을 반전
  - grayscale: `255 - img`
  - color: `Scalar(255,255,255) - img`
- **Gamma Correction**
  - LUT(Look-Up Table)를 사용하여 gamma 변환 수행

### 5. Thresholding
Thresholding 메뉴에서 다음 모드를 지원합니다.

- `THRESH_BINARY`
- `THRESH_BINARY_INV`
- `THRESH_TRUNC`
- `THRESH_TOZERO_INV`
- `THRESH_TOZERO`

추가 옵션:
- Threshold value를 spinner로 수동 조절 가능

---

## GUI Structure

프로그램은 GLUI 메뉴를 통해 기능을 제어합니다.

- **READ**
  - 이미지 파일 불러오기
- **Cropping**
  - 마우스로 ROI 선택
- **Save**
  - 현재 결과 저장
- **Drawing Panel**
  - PEN / LINE / RECTANGLE / CIRCLE
  - THICK / RGB / Fill
- **Processing Panel**
  - LINEAR CONTRAST
  - INVERSE
  - GAMMA CORRECTION
  - THRESHOLDING

---

## How to Use

### 1. 이미지 불러오기
1. 프로그램 실행
2. `READ` 버튼 클릭
3. 이미지 선택

### 2. 드로잉
1. Drawing 메뉴에서 도형 선택
2. 선 두께와 RGB 값 조절
3. 마우스로 이미지 위에 직접 그림

### 3. 크롭
1. `Cropping` 버튼 클릭
2. 이미지 창에서 드래그하여 영역 선택
3. 선택한 영역이 현재 이미지로 반영됨
4. `ESC` 키로 크롭 모드 종료

### 4. 영상 처리
1. Processing 메뉴에서 기능 선택
2. 필요한 파라미터 조정
3. 결과가 이미지에 반영됨

### 5. 저장
1. `Save` 버튼 클릭
2. 파일명 입력 후 저장

---

## Core Implementation Details

### Cropping
`setMouseCallback("img", onMouse)`를 사용하여 마우스 이벤트를 처리합니다.

- `EVENT_LBUTTONDOWN`: 시작 좌표 저장
- `EVENT_MOUSEMOVE`: 드래그 중 사각형 미리보기
- `EVENT_LBUTTONUP`: ROI 추출 후 현재 이미지 갱신

### Pen Drawing
마우스를 누른 상태에서 이동하면 현재 위치에 작은 원을 계속 찍는 방식으로 구현했습니다.

### Line / Rectangle / Circle
- 마우스 다운 시 시작점 저장
- 마우스 업 시 끝점 저장
- 시작점과 끝점을 이용해 도형 생성

### Linear Contrast
직선 구간 변환 함수 `contrastEnh()`를 이용하여 각 픽셀 값을 piecewise linear 방식으로 변환합니다.

### Gamma Correction
256 크기의 LUT를 생성하고 `LUT()` 함수로 빠르게 적용합니다.

### Thresholding
OpenCV의 `threshold()` 함수를 이용하여 각 모드를 적용합니다.

---

## Environment

- Language: **C++**
- GUI: **OpenGL, GLUI**
- Image Processing: **OpenCV**
- OS: **Windows** 기반 개발

---

## Build / Run

프로젝트는 Visual Studio + Windows 환경에서 작성되었습니다.  
실행을 위해 다음 라이브러리가 필요합니다.

- OpenCV
- OpenGL
- GLUI

예시:
- Visual Studio에서 프로젝트 열기
- OpenCV include/lib 설정
- GLUI 및 OpenGL 링크 설정 후 실행

---

## Assignment Coverage

과제 요구사항 기준으로 코드에서 확인되는 구현 내용은 다음과 같습니다.

### 공통사항
- [x] Read Button으로 이미지 불러오기
- [x] Save Button으로 결과 저장
- [x] 컬러/그레이 영상 처리 가능하도록 작성됨

### 그림 그리기
- [x] 펜
- [x] 선
- [x] 사각형
- [x] 원
- [x] 색 조절
- [x] 선 두께 조절
- [~] 채워넣기 일부 지원 (`CIRCLE` 중심)

### Cropping
- [x] 마우스로 영역 선택 후 저장 가능

### Contrast
- [x] Linear Contrast
- [x] Inverse
- [x] Gamma Correction
- [ ] 그래프 출력은 코드상 명확히 확인되지 않음

### Thresholding
- [x] 5가지 모드
- [x] 임계값 조정
- [~] 컬러/그레이 처리 시도
- [ ] Otsu는 현재 코드에 구현되어 있지 않음

---

## Limitations

현재 코드 기준으로 다음과 같은 한계가 있습니다.

- `Otsu Thresholding` 미구현
- `Contrast graph` 출력 미구현
- `Rectangle Fill`은 체크박스가 있어도 실제 적용 코드가 보이지 않음
- 일부 전역 변수명 오탈자 존재
  - 예: `RECANGLE`, `draw_Rectangel`
- 일부 패널 enable/disable 로직이 정리되지 않음
- 크롭 시 좌표 방향에 따라 ROI 예외가 발생할 수 있음
- 원 그리기는 실제로는 타원보다 원 중심 구현에 가까움

---

## Possible Improvements

- Otsu Thresholding 추가
- Rectangle / Ellipse Fill 완전 지원
- Contrast transformation graph 시각화
- 예외 처리 강화
  - 잘못된 ROI 선택
  - 이미지 미로드 상태에서 기능 실행
- 미리보기 기능 개선
- 코드 리팩토링
  - 전역 변수 축소
  - 클래스 구조화
  - 함수명/변수명 정리

---

## Author

학부 컴퓨터비전 수업 기말과제로 작성한 프로젝트입니다.
