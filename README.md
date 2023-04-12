# 아두이노용 폭탄 해체 시뮬레이션

#### 사용 기술: visualMicro
#### 사용 부품
- 키패드
- 7-segment(타이머용, x4,x1)
- 가변저항
- 8x8 dot matrix
- 아두이노 mega
- led bar
- 부저

<img width="30%" src="https://user-images.githubusercontent.com/33209821/230916861-406db3f1-f681-4ec8-a54c-6b774386ee5f.jpg"/>
<img width="80%" src="https://user-images.githubusercontent.com/33209821/230917444-256cd88d-b5a4-47fe-96e9-5578be910ad4.png"/>

### 패턴에 대한 자세한 설명은 첨부된 ppt 파일에 서술됨

#### 설명

- 시작 시 폭탄의 제조사, 버전, 시리얼 번호가 랜덤하게 출력된다. 해당 정보를 가지고 게임을 플레이 한다.
- 시간은 5분이 주어진다.
- 시간이 1분 미만으로 남았을 시 분 영역에 초를 표기한다.
```C++
if (minute == 0) {
				alert_mode = true;
				minute = 59;
				second = 59;
			}
```
- 섹터는 총 6개로 나눠져 있으면 푸는 순서는 폭탄의 종류에 따라 다르다.(마지막 순서 제외)
```C++
byte total_order[5];// 마지막 순서는 항상 정해져 있기에 배열크기는 5개
byte current_order_index = 0;
```
- 기회는 총 3번 주어지며 실패 할 때마다 시간은 빠르게 줄어든다.
```C++
fail_count++;
current_order_index++;// 순서 넘어감
total_order_check();
timer_gap1 -= 200;// 프레임 업데이트 속도 증가
second_gap += 0.25;// 시간 감소 폭 증가
score -= 2000;// 점수 차감
```
- 디지털핀이 부족하여 버튼에 쓰인는 핀들은 슬라이드 스위치를 통해 재사용
- 섹터 3번 중 가변저항의 출력 값을 맵핑하여 사용
```C++
row = map(analogRead(A12), 1025, -10, 0, 8);
col = map(analogRead(A13), 1025, -10, 0, 8);
```


#### 피드백
- 구조가 너무 복잡하여 회로도 구성에 어려움이 있고 하드웨어의 유지보수가 어려움
- 버튼의 글리치 처리 필요
- 코드 파일을 나눴으면 가독성이 좋아졌을 듯
