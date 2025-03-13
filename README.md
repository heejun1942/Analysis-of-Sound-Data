# 음성 데이터 분석

## 1. 음성 데이터란?



### 1) 소리(Audio)의 이해
- 어떤 물체가 진동하면서 발생
- 목소리의 경우 공기 분자가 진동을 하면서 발생 
- 즉 매질인 공기 분자가 얼마나 크게 흔들렸는지에 따라 형성되는 이러한 공기압의 진폭이, waveform 형태를 띄게 되어 우리가 흔히 보는 그래프가 그려짐

![Waveform(x축은 시간, y축은 진폭폭)](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbrhmQX%2FbtqC7rOEtmz%2FAKY5xZ3FcQDimiewJLYKD0%2Fimg.png)

- 소리의 3요소: 세기, 높낮이, 음색

#### (1) 진폭(Amplitude)
- 소리의 크기와 관련


#### (2) 주파수(frequency)
- 소리의 높낮이와 관련
- 주파수는 Hz(헤르츠)라는 단위를 사용하며, 단위시간 1초에 진동하는 횟수

- *[참고] 사람의 가청주파수 대역은 20Hz~20KHz*


#### (3) 음색(Tone Color, Timbre)
- 기음과 배음의 구성이 음색을 결정짓는 중요한 요소
    - 기음/기본 주파수 : 소리의 높낮이를 구분할 수 있는 기본이 되는 주파수
    - 배음 / 고조 주파수 : 기음의 정수배가 되는 주파수



#### (4) 위상(Phase)
파동이 현재 어떤 상태(위치)에 있는지를 나타내는 개념

- 예시: 그네 타기  
    그네를 탈 때, 맨 앞까지 갔다가 다시 뒤로 가는 걸 반복하지?

    - 맨 앞: 위상이 0도 (출발점)
    - 중간: 위상이 90도 (절반 지남)
    - 맨 뒤: 위상이 180도 (반대편 도착)

### 2) 샘플링(Sampling) & 샘플 레이트(Sample rate)
- 컴퓨터가 이해할 수 있도록 소리(아날로그 신호)를 숫자(디지털)로 변환하는 과정
- 이때 신호를 측정하는 간격을 샘플 레이트(Sample rate)라고 함

- *[참고] 일반적으로 표준 샘플링 레이트는 44.1KHz*


### 3) windowing


## 2. 음성데이터 특징 추출(Feature extraction)

### 푸리에 변환 (Fourier Transform)
- 푸리에 변환(Fourier Transform)은 복잡한 파동을 여러 개의 단순한 파동(주파수 성분)으로 나누는 방법

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FboqscG%2FbtqCUxBxL6A%2Fkp3sBQqU6cjwdnzeNWKXmK%2Fimg.png)

- 푸리에 변환을 하면 시간 영역(Time domain)이 사라짐

![푸리에 변환](https://blog.kakaocdn.net/dn/b3FgvC/btqCZyFlFp8/Do5RduBzXBZAcYxf8iRab0/img.gif)

- 전체 waveform에 한 번에 변환을 취할 수도 있겠지만, 자른(split)한 프레임(frame) 마다 적용하는 Short Time Fourier Transfrom(STFT) 방법이 있음

## 3. 특징 추출 기법: 스펙트럼/ 멜 스펙토그램/ MFCC

### 1) 스펙트럼(Spectrum)

- 푸리에 변환을 실시한 결과를 그래프로 나타낸 것
- 파동의 시간 영역(Time domain)을 주파수 영역(Frequency domain)으로 변환

![푸리에 변환](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcmRjaQ%2Fbtq1NttQzq4%2FmwC4GjFdH4pn6gGFVlxLpk%2Fimg.png)

- 음향 신호를 주파수, 진폭으로 분석하여 보여준다(시간에 대한 정보가 없다)
- x축은 주파수(Frequency), y축은 진폭(Amplitude)을 나타낸다

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmYh97%2Fbtq1MqD7BEU%2FwK4UkF5ZcLvTkupjdFjKkk%2Fimg.png)

### 2) 멜 스펙트로로그램(Mel Spectrogram)

#### (1) 멜-스케일(mel-scale)  

- 왜 필요할까?
    - 사람의 귀는 낮은 주파수는 민감하게, 높은 주파수는 둔감하게 들음
    - 일반적인 주파수 스펙트럼(푸리에 변환 결과)에서는 고주파 영역이 너무 세밀해서 사람이 듣는 감각과 맞지 않음
    - 따라서 멜(Mel) 스케일을 사용해 사람이 듣는 방식에 가깝게 변형한 게 멜스펙트럼!

- 예시
    - 일반 주파수 스펙트럼 → 100Hz, 200Hz, 300Hz, ... 이런 식으로 일정한 간격
    - 멜스펙트럼 → 100Hz, 150Hz, 250Hz, 400Hz, ... 낮은 주파수는 촘촘, 높은 주파수는 넓게

- Mel Filter Bank
    - 1000Hz까지는 Linear하게 변환하다가 그 이후로는 Mel scale triangular filter를 만들어 곱해줌
    - 보통 26개 혹은 40개 정도의 filter bank를 사용
    - 각 Filter Bank 영역대 마다 Energy값(spectrum power값 평균)을 모두 합하고 log를 취해줌
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbPy60k%2FbtqCUx9lJ88%2FYWGYhrOK1ZFxeOuaWgCxu1%2Fimg.png)

#### (2) 스펙트로그램(Spectrogram)  
- 시간에 따른 주파수 변화   
- 여러 개의 스펙트럼을 시간 축을 따라 쌓아서 만든 그림

![스펙트로그램](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FErVIg%2FbtqCX9sstFA%2FyCH9xZiePiYwqSoPmFA78K%2Fimg.png)


#### 📢 스펙트럼(Spectrum) vs. 스펙트로그램(Spectrogram)  
- 둘 다 주파수(소리의 높낮이)와 관련된 그래프지만, 시간을 포함하는지 여부가 핵심 차이!


| 비교 항목  | 스펙트럼 (Spectrum) | 스펙토그램 (Spectrogram) |
|------------|---------------------|-------------------------|
| 의미       | 특정 순간의 주파수 성분 | 시간에 따른 주파수 변화 |
| x축       | 주파수 (Hz)         | 시간 (s)               |
| y축       | 진폭 (에너지 크기)   | 주파수 (Hz)             |
| 추가 정보  | 시간 정보 없음       | 색깔로 진폭(에너지) 표현 |


#### (3) 멜 스펙트로그램 (mel spectrogram) 이란?  
- 멜 스펙토그램은 시간에 따른 멜 스펙트럼의 변화를 시각적으로 표현한 것

- 일반적인 스펙토그램은 푸리에 변환(FFT)으로 만든 주파수 스펙트럼의 시간 변화를 보여줘.
- 하지만 멜 스펙토그램은 여기서 한 단계 더 나아가 **멜 스케일(Mel Scale)**을 적용해서 사람이 듣는 방식에 가깝게 변형한 거야.

🤖 딥러닝에서 왜 중요할까?
- 멜 스펙토그램은 음성·소리 데이터를 딥러닝 모델이 쉽게 이해할 수 있는 이미지 형태로 변환하는 기술이야!
- 특히 CNN 같은 이미지 기반 모델을 활용할 때 필수적인 입력 데이터로 많이 쓰여! 


### 3) MFCC(Mel Frequency Cepstral Coefficients)
-  Mel Spectrum(멜 스펙트럼)에서 Cepstral(켑스트럴) 분석을 통해 추출된 값

- MFCC 추출과정
    - 프레임 분할 (Frame splitting): 음성신호를 작은 프레임(20~40ms)으로 나누기 
    - 푸리에 변환 (FFT): 각 프레임에 푸리에 변환을 적용하여 스펙트럼(주파수 정보를 가짐)을 얻음
    - 멜 스케일(Mel scale) 적용  
    - Cepstral 분석: Log(로그 변환) + IFFT(Inverse FFT, 역 고속 푸리에 변환)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FchQNK5%2FbtqCZZ3CW8s%2Fx5zR5q2bow8rUjx2cfDV5K%2Fimg.png)

#### (1) Cepstral 분석

- 어떻게 Spectrum에서 소리의 고유한 특징을 추출할 수 있을까?
    - 사람의 음성은 일반적으로 배음(harmonics) 구조를 가지고 있음
    - 배음 구조의 차이가 음색의 차이를 만듬
    - Spectrum에서 배음 구조를 유추해낼 수 있다면 소리의 고유한 특징을 찾아낼 수 있는 것
    - 이것을 가능하게 하는 것이 Ceptral 분석!

#### (2) DCT(Discrete Cosine Transform)
- DCT는 특정 함수를 cosine 함수의 합으로 표현하는 변환
- 이때 앞쪽(low) cosine 함수의 계수가 변환 전 데이터의 대부분의 정보를 가지고 있고 뒤쪽으로 갈수록 0에 근사해 데이터 압축의 효과를 보인다. 즉 낮은 주파수 쪽으로 에너지 집중현상이 일어난다.
- MFCC는 이 계수(cepstrum coefficient) 중 주파수가 낮은, 정보와 에너지가 몰려있는 12개의 계수(cepstrum coefficient)를 선택해 이를 feature로 사용한다. 그리고 이 12개 계수에 해당하는 각 frame의 Energy를 13번째 feature로 사용한다. cepstrim coefficient는 다음과 같이 구해진다.
- 즉,  Mel-spectrogram에 DCT를 취하고 낮은(정보가 많은) 계수(cepstrum coefficient) 12개와, 이들로 구해진 energy를 더해 한 frame 당 총 13개의 값이 feature 역할을 하게 되고 이를 MFCC(mel frequency cepstrum coefficient)라고 한다. 




#### 📢 멜 스펙트로그램(mel spectrogram) vs MFCC(Mel Frequency Cepstral Coefficients)
> Mel-Spectrogram은 주파수끼리 상관성(Correlation)이 형성되어 있는데, 이러한 상관관계를 De-Correlate해줌

| 비교 항목 | Mel Spectrogram | MFCC |
|------------|---------------------|---------------------|
| 출력 형태 | 이미지 형태 (시간-주파수 변화를 시각적으로 표현) | 수치 형태 (특징을 나타내는 계수) |  
| 목적 | 시간-주파수 변화 시각화 (주로 분석과 시각화) | 음성 특징을 추출하여 인식, 분류에 사용 |
| 처리 과정 | 푸리에 변환 → 멜 스케일 변환 → 스펙트럼 시각화 | 푸리에 변환 → 멜 스케일 변환 → 로그 변환 → DCT |
| 사용 용도 | 주로 음성 분석, 음악 분석  | 음성 인식, 감정 분석, 음성 합성 등 |

### 출처
- ChatGPT
- https://www.youtube.com/watch?v=YQxO-i4A4gQ
- https://sswwd.tistory.com/2
- https://www.youtube.com/watch?v=Z_6tAxb89sw
- https://brightwon.tistory.com/11
- https://hyunlee103.tistory.com/54