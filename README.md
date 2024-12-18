<한국어- Korean version> 

# 주제: CCTV기반 주취자 행동 감지 및 음주운전 예방 AI System 
	 총 3가지 형식의 모델로 주취자 식별을 진행한다

# <모델>
모델 1. Detection모델 

	Class: Car, Open Driver-seat, Car-Door Open, Person 을 통해 사람이 자동차에 접근하였을 때, 운전석으로 향하는지, 혹은 다른좌석으로 항하는지 확인 할수있다.

모델 2. Yolo Track & Pose Estimation Tracking 모델
	
 	1. 각 사람마다 ID를 부여
  	2.Tracking에 의해 생성된 ID의 Bbox를 기준으로 포즈 측정을 진행:
   	 * 각 프레임 중 각 주체의 BBOX를 제외한 부분을 색상을 검정색으로 칠해 더 자세하게 식별
     * 각 프레임과 각 주체마다 포즈 측정을 진행 한 뒤, 하나의 csv 파일에 저장한다.

모델 3. Sequence (LSTM 혹은 Transformer) model2를 통해 수집된 csv 파일을 기반으로 Sequence length를 90으로 지정한 뒤 연속적인 값에 대해 값을 정하기 위해 Sequence model(LSTM 혹은 Transformer)을 통해 진행된다.

# <시스템 Workflow> 
![스크린샷 2024-12-10 오전 11 10 37](https://github.com/user-attachments/assets/dfcb481b-299b-45d0-b99b-88bfdbd34fff)

위 모델 3개를 통해 주취자 식별 및 음주운전 조기 포착을 진행하는데,
1. 우선 Sequence model 을 통해 주취자를 식별 후 주취자가 움직일때마다 빨간색으로 주취자를 Bbox를 그린다.
2. 모델 1으로 포착한 자동차와 거리가 가까워지면 자동차의 라벨을 Intoxicated person car로 변경한다.
3. 그 뒤, 총 3개의 Class인 Intoxciated car ,open driver-seat, drunken person이 겹치게 되면 분홍색으로 화면색상을 전환하여 위험성을 부여하고
4. 탑승 후 자동차의 Center값의 위치가 이전 프레임에서 200픽셀 이상 이동한다면 화면 색깔을 빨간색으로 변환하여 경고를 준다.

모델을 작동시키기 위해 해당 깃허브를 다운로드 받은 후, main.py를 통해 모델을 실행시키면 됩니다.

욜로 모델 다운로드 받기 위해서 아래의 드라이브를 통해 다운받아야 합니다.

욜로 10 Xlarge

	https://drive.google.com/file/d/1B7k7i9CIquFpnnEZigEpRNejrcJ6hFxc/view?usp=sharing

욜로 8 large

	https://drive.google.com/file/d/1SmdWKzg9xhvwNPjjC-D9puSyNi35Jjmz/view?usp=sharing

<English - 영어 version>
# Topic: CCTV-based AI System for Identifying Intoxicated persons and Preventing Drunk Driving
  	the system employes 3 models to detect and identify intoxicated persons: 

# <Models> 
Model 1 : Detection Model

  	-Clasees: Car, Open_driver_seat, Car_door_open, Person, Motocycle
  	-Purpose: Detects human interation with vehicles, identifying if the person approaches the driver's seat or another seats. 

Model 2: Yolo Track & Pose Estimation Model 

 	-Assigns unique ID to persons using tracking features. 
  	-Tracks each individual using bounding boxes (BBoxes) generated by their IDs. To enhance pose estimation accuracy;
   		* The non-BBox areas in each frame are filled with black, isolating the subject. 
    	* Conducts pose estimation for each individual in each frame. 
    	* Outputs the results into CSV file. 

Model 3: Sequence Model (LSTM or Transformer) 

  	-Utilizes the CSV file from Model2 to process sequential data with a sequence length of 90 frames.
  	- Predicts patterns in to continuous motion data using Sequntial Model architecture( LSTM or Transformer)
  	 (* If you desire to change the model, you may change the "infer"part in "main.py")

# <Identification Workflow> 
1. Drunk person Identification: 
  -Sequence model detects intoxicated behavior. Identified individuals are marked with a "Red Bounging Box"
2. Proximity Alert:
  -If a drunk person moves closer to a car detected by Model 1, the car label changes to "Intoxicated Person Car"
  and the color of monitor will changes into "Yellow Color" 
3. Risk Escaltion Alert:
  -If the classes Intoxicated Person Car, Open_driver_seat, and Drunken Person overlap, the screen chaenges to "Pink"
  ,signaling a high-risk situation. 
4. Movement Alert:
  -After a drunk person enters the car, if the car's center position shifts by more than "200 pixels" in the next frame,
  the screen shows "Red Color", issuing a critical warning. 

<Execution Instructions>
  -Download the repository from GitHub.
  -Run the system by executing main.py 
  -Download the Yolo model weights from the provided drive link.

This system aims to preemptively detect intoxicated individulas and intervene in potential drunk driving scenarios.
