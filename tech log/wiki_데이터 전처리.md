# :scissors:데이터 전처리

> 분류할 감정 `class`를 정했고, 라벨링 작업까지 끝냈다면 그 다음 과정은 데이터 전처리이다. 앞에서 데이터 전처리를 하는 것은 수능을 앞두고 족집게 모의고사 문제를 만드는 것과 비슷한 것이라고 말한 적이 있다. 우리는 노트북에 일반적으로 들어있는 웹캠을 이용해 얼굴의 사진을 받아들일 것이기에 어떤 이미지가 들어올지는 대강 예측할 수 있다. 준비된 dataset을 그와 비슷한 각도, 조명 등의 조건에서 유사한 이미지들을 골라 더욱 정제된 dataset을 만드는 것이다.



하지만, 여기서 한 가지를 더 고려해야 한다. 

우리가 하고 싶은 것은 얼굴 이미지를 통한 감정 분류이다.   중요한 것은 '얼굴', 그 중에서도 '표정'이다. 해당 이미지의 얼굴 부분만 잘라내어도 모델을 훈련시킬 수 있으며 효율적인 연산을 할 수 있다. 그렇기에 이하와 같은 데이터 전처리 과정들을 거쳤으며, 해당 과정들 중 일부는 모델링 실험과 병행하여 진행하였다. 



## 0. 데이터 전처리 과정

![이미지 전처리하기](https://user-images.githubusercontent.com/58945760/88071416-bfbffd80-cbae-11ea-9c5a-882d1a9b553f.PNG)

## 1. 배경 제거

>  우선은 OpenCV를 통해 공통 좌표를 찍어 선택 영역을 지정하는 방식으로 원본 사진에서 필요없는 배경을 제거하였다. 

![](https://github.com/k-face/k-face_2019/raw/master/image/Amount_of_the_data.png)

  배경이 많으면 많을수록 빠른 연산에 방해가 된다. K-FACE dataset의 경우 한 사람을 찍은 이미지 데이터가 각도, 조명, 악세사리 착용 유무 등에 따라 여러 폴더로 복잡하게 나뉘어 있었으므로 데이터를 한 곳에 모아놓을 필요성이 있었다. 

때문에 코드로 데이터를 한 곳에 모으는 동시에 OpenCV를 통해 불필요한 배경들을 최대한 잘라내는 작업을 진행하였다. 



## 2. 얼굴 검출 & 2차 배경 제거

> 사실상 이 부분부터가 진짜 데이터 전처리라고 할 수 있다. 배경을 제거했지만 여전히 감정 분류에 있어 필요 없는 부분이 남아 있다. 이를 해결하기 위해 `Cascade classifier`를 사용하여 얼굴 부분만 검출한 후 잘라내었다. 



## 3. `class`별 이미지 전처리

> 얼굴 검출 후에는 전처리의 마지막 과정으로, 각 `class` 폴더 내 특성이 충분히 추출되지 않는 이미지들을 제외하였다. 



 원래는 모든 `class`가 보다 넓은 범위의 특징을 학습하여 분류를 할 수 있도록 할 생각이었으나, 실제로 모델링을 시도해본 결과 `class`별 특성을 설정하여 전처리를 진행하는 것이 보다 높은 정확도를 가져올 수 있다는 사실을 알았다.  각 표정 `class`별 특성은 다음과 같다.



- **Neutral** : 입꼬리가 내려가거나 올라가지 않고, 미간에 힘이 들어가지 않음. 눈을 감지 않았음.
- **Positive** :  웃을 때 이빨이 보임. 전체적으로 역동적인 표정. 활짝 웃는 얼굴.  
- **Negative** : 미간의 찡그림. 전체적으로 역동적인 표정. 양 입 끝이 아래로 향하거나, 입술이 정면으로 튀어나옴. 



이러한 작업은 모든 팀원이 참여하여 수작업으로 진행되었고, 정확도 향상에 기여할 수 있었다. 