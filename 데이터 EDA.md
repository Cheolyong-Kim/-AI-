# EDA

<br>

[데이콘 링크](https://dacon.io/competitions/open/235576/overview/description)

데이콘 교육탭에서 확인할 수 있는 서울시 따릉이 자전거 이용 예측 AI모델을 만들어볼 것이다.

각 날짜의 1시간 전의 기상상황을 활용하여 따릉이 대여수를 예측하는 것이 목표이다.

<br>

데이터의 속성들은 모두 id, hour, temperature, precipitation, windspeed, humidity, visibility, ozone, pm10, pm2.5, count이다.

속성들은 각각 아래의 의미를 가진다.

- id 고유 id
- hour 시간
- temperature 기온
- precipitation 비가 오지 않았으면 0, 비가 오면 1
- windspeed 풍속(평균)
- humidity 습도
- visibility 시정(視程), 시계(視界)(특정 기상 상태에 따른 가시성을 의미)
- ozone 오존
- pm10 미세먼지(머리카락 굵기의 1/5에서 1/7 크기의 미세먼지)
- pm2.5 미세먼지(머리카락 굵기의 1/20에서 1/30 크기의 미세먼지)
- count 시간에 따른 따릉이 대여 수

<br>

우선 EDA를 통해 데이터 피처들과 count의 상관 관계를 알아본다.

<br>

---

![j1](https://github.com/Cheolyong-Kim/DACON-Prediction_of_Ttareungi_Bicycle_Use/blob/master/image%201/j1.png?raw=true)

train 데이터를 불러온다.

<br>

![j2](https://github.com/Cheolyong-Kim/DACON-Prediction_of_Ttareungi_Bicycle_Use/blob/master/image%201/j2.png?raw=true)

결측치를 확인해보니 결측치가 꽤 존재한다. 우선은 0으로 채워준다.

<br>

![j3](https://github.com/Cheolyong-Kim/DACON-Prediction_of_Ttareungi_Bicycle_Use/blob/master/image%201/j3.png?raw=true)

![j4](https://github.com/Cheolyong-Kim/DACON-Prediction_of_Ttareungi_Bicycle_Use/blob/master/image%201/j4.png?raw=true)

히트맵을 통해 속성들간의 관계를 쉽게 알 수 있게 한다. 하얀색에 가까울 수록 상관관계가 크다는 의미이다.

count 속성과 다른 속성들을 봤을 때 hour, hour_bef_temperature, hour_bef_windspeed, hour_bef_visibility, hour_bef_ozone 정도가 상관이 있는 것으로 보인다.

<br>

![j5](https://github.com/Cheolyong-Kim/DACON-Prediction_of_Ttareungi_Bicycle_Use/blob/master/image%201/j5.png?raw=true)

![j6](https://github.com/Cheolyong-Kim/DACON-Prediction_of_Ttareungi_Bicycle_Use/blob/master/image%201/j6.png?raw=true)

pairplot으로 속성별 분포를 나타낸다. 데이터 속성이 많아서 끊어서 표현한다.

위에서 찾은 속성들이 대체로 count 속성에 대해 회귀선을 그을 수 있게 분포하고 있는 것을 확인할 수 있다.

hour_bef_precipitation 속성은 0과 1로 구분되어 있어 따로 확인한다.

<br>

![j7](https://github.com/Cheolyong-Kim/DACON-Prediction_of_Ttareungi_Bicycle_Use/blob/master/image%201/j7.png?raw=true)

![j8](https://github.com/Cheolyong-Kim/DACON-Prediction_of_Ttareungi_Bicycle_Use/blob/master/image%201/j8.png?raw=true)

hour_bef_visibility가 약간 애매한 분포를 보여주지만 피처의 다양성을 위해 남겨두기로 결정했다.

<br>

![j9](https://github.com/Cheolyong-Kim/DACON-Prediction_of_Ttareungi_Bicycle_Use/blob/master/image%201/j9.png?raw=true)

countplot을 활용하여 hour_bef_precipitation 속성의 각각 count 수를 확인한다.

hour_bef_precipitation 속성이 0.0이면 count 값이 크게 증가하는 것을 볼 수 있지만 이 문제는 회귀문제이기 때문에 이 속성은 사용하지 않기로 결정했다.

<br>

![j10](https://github.com/Cheolyong-Kim/DACON-Prediction_of_Ttareungi_Bicycle_Use/blob/master/image%201/j10.png?raw=true)

![j11](https://github.com/Cheolyong-Kim/DACON-Prediction_of_Ttareungi_Bicycle_Use/blob/master/image%201/j11.png?raw=true)

![j12](https://github.com/Cheolyong-Kim/DACON-Prediction_of_Ttareungi_Bicycle_Use/blob/master/image%201/j12.png?raw=true)

![j13](https://github.com/Cheolyong-Kim/DACON-Prediction_of_Ttareungi_Bicycle_Use/blob/master/image%201/j13.png?raw=true)

![j14](https://github.com/Cheolyong-Kim/DACON-Prediction_of_Ttareungi_Bicycle_Use/blob/master/image%201/j14.png?raw=true)

학습에 사용하기로 한 속성들의 이상치를 확인하기 위해 boxplot을 활용한다. 이상치가 0인 속성은 실제 데이터 전처리를 할 때 중앙값이나 평균값으로 채우면 되기 때문에 크게 신경쓰지 않는다.

이상치가 boxplot의 최대값보다 큰 값들은 실제 데이터 전처리에서 boxplot의 최대값이 되도록 해줄 것이다.