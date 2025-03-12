# 음성 데이터 분석

## 1. 음성 데이터란?



### 1) 소리의 구성

#### (1) 진폭(Amplitude)
- 소리의 크기와 관련



#### (2) 주파수(frequency)
주파수는 Hz(헤르츠)라는 단위를 사용하며, 단위시간 1초에 진동하는 횟수

[참고] 사람의 가청주파수 대역은 20Hz~20KHz  


### 2) 샘플링(Sampling)?
- 컴퓨터가 이해할 수 있도록 소리(아날로그 신호)를 숫자(디지털)로 변환하는 과정
- 이때 신호를 측정하는 간격을 **샘플 레이트(Sample Rate)**라고 함

[참고] 일반적으로 표준 샘플링 레이트는 44.1KHz


## 2. 음성데이터 특징 추출(Feature extraction)

### 푸리에 변환 (Fourier Transform)


## 3. 특징 추출 방법: 스펙트럼/ 멜 스펙토그램/ MFCC

### 1) 스펙트럼

### 2) 멜 스펙토그램

#### (1) 멜-스케일(mel-scale)  

- 왜 필요할까?
    - 사람의 귀는 낮은 주파수는 민감하게, 높은 주파수는 둔감하게 들음
    - 일반적인 주파수 스펙트럼(푸리에 변환 결과)에서는 고주파 영역이 너무 세밀해서 사람이 듣는 감각과 맞지 않음
    - 따라서 멜(Mel) 스케일을 사용해 사람이 듣는 방식에 가깝게 변형한 게 멜스펙트럼이야!

- 예시
    - 일반 주파수 스펙트럼 → 100Hz, 200Hz, 300Hz, ... 이런 식으로 일정한 간격
    - 멜스펙트럼 → 100Hz, 150Hz, 250Hz, 400Hz, ... 낮은 주파수는 촘촘, 높은 주파수는 넓게

#### (2) 스펙토그램(Spectogram)  
- 시간에 따른 주파수 변화   
- 여러 개의 스펙트럼을 시간 축을 따라 쌓아서 만든 그림

<br>

📢 스펙트럼(Spectrum) vs. 스펙토그램(Spectogram)  
- 둘 다 주파수(소리의 높낮이)와 관련된 그래프지만, 시간을 포함하는지 여부가 핵심 차이!


| 비교 항목  | 스펙트럼 (Spectrum) | 스펙토그램 (Spectrogram) |
|------------|---------------------|-------------------------|
| 의미       | 특정 순간의 주파수 성분 | 시간에 따른 주파수 변화 |
| x축       | 주파수 (Hz)         | 시간 (s)               |
| y축       | 진폭 (에너지 크기)   | 주파수 (Hz)             |
| 추가 정보  | 시간 정보 없음       | 색깔로 진폭(에너지) 표현 |


#### (3) 멜 스펙토그램 (mel spectogram) 이란?  
- 멜 스펙토그램은 시간에 따른 멜 스펙트럼의 변화를 시각적으로 표현한 것

- 일반적인 스펙토그램은 푸리에 변환(FFT)으로 만든 주파수 스펙트럼의 시간 변화를 보여줘.
- 하지만 멜 스펙토그램은 여기서 한 단계 더 나아가 **멜 스케일(Mel Scale)**을 적용해서 사람이 듣는 방식에 가깝게 변형한 거야.

🤖 딥러닝에서 왜 중요할까?
- 멜 스펙토그램은 음성·소리 데이터를 딥러닝 모델이 쉽게 이해할 수 있는 이미지 형태로 변환하는 기술이야!
- 특히 CNN 같은 이미지 기반 모델을 활용할 때 필수적인 입력 데이터로 많이 쓰여! 


### 3) MFCC




### 출처
- https://www.youtube.com/watch?v=YQxO-i4A4gQ
- https://sswwd.tistory.com/2
- ChatGTP
- https://www.youtube.com/watch?v=Z_6tAxb89sw