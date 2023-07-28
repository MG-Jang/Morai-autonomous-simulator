# Publish 와 subscribe

publish(발신자) 와 subscribe(수신자) 로 구별하여 데이터를 주고 받는 형태이다. 아래 예시를 보며 더 알아보자.

> talker, listener 통신

- <span style="color:orange">아래 두명령어는 터미널을 새로 열때 마다 입력을 해주어야 한다. <br>
(실행 위치는 상관X)
```py
# catkin 환경 변수 선언
$ source ~/catkin_ws/devel/setup.bash
#패키지 재구축 
$ rospack profile
```

- talker, listener실행

```py 
# 터미널 1: 마스터 노드 실행 (ROS 실행)
$ roscore

# 터미널 2: talker 실행 ()
$ roscd beginner_tutorials/scripts
$ chmod +x talker.py   #코드의 실행 권한을 주겠다는 뜻 
$ rosrun beginner_tutorials talker.py

# 터미널 3: listener 실행
$ roscd beginner_tutorials/scripts
$ chmod +x listener.py
$ rosrun beginner_tutorials listener.py
```

- 통신 실행결과

![LYNMP 로고](https://github.com/MG-Jang/img/blob/main/talkerlistener.JPG?raw=true "LYMNP 로고")

> talker listener 통신 코드 분석

코드의 일부분만 가져온 것으로 전체 코드를 확인하려면 ssafy_1 과제 완성 폴더를 확인해 볼것

- student.msg (아래 import 한 내용)
```msg
string first_name
string last_name
uint8 age
uint32 score
```

- my_name_talker.py
```py
import rospy  # ROS를 사용
from std_msgs.msg import String
from ssafy_1.msg import student  # student를 가져옴, msg는 전송한 문자의 자료형을 나타내는 msg 형태의 파일

def talker():
    #TODO: (1) publisher 생성
    # student 라는 직접 만든 Custom ROS 메세지 형식을 사용하여 Topic Publisher 를 완성한다.
    # Topic 이름은 'my_name' 으로 설정한다.
    publisher = rospy.Publisher( "my_name" , student , queue_size=10)

    #TODO: (2) ROS 노드 이름 선언
    rospy.init_node('my_name_talker', anonymous=True)

    count = 0

    #TODO: (3) 코드 반복 시간 설정 및 반복 실행    
    rate = rospy.Rate(1) # 1 hz , hz는 초당 몇 회진동으로 높을 수록 빠르게 데이터를 전송한다.
    while not rospy.is_shutdown():
        #TODO: (4) 송신 될 메세지 변수 생성 및 터미널 창 출력 
        # 송신 될 메세지 변수를 만든뒤 출력 결과를 확인한다.        
        my_name = student()   # 위에서 student를 가져왔으므로 student로 선언
        #아래는 형식에 맞게 작성
        my_name.first_name = "Jang" 
        my_name.last_name =  "MG"
        my_name.age = 27
        my_name.score = 100
        rospy.loginfo('\n my name : %s %s \n my age : %i \n SSAFY score : %i', my_name.first_name,my_name.last_name,my_name.age,my_name.score)
        
        
        #TODO: (5) /my_name 메세지 Publish 
        publisher.publish(my_name)
        
        count += 1
        rate.sleep()

if __name__ == '__main__':
    try:
        talker()
    except rospy.ROSInterruptException:
        pass
```
- my_name_listener.py
```py
import rospy
from std_msgs.msg import String
from ssafy_1.msg import student

# my_name_listener 는 Custom Msgs 를 이용한 Topic Subscriber(메세지 수신) 예제입니다.
# /my_name 라는 메세지를 Subscribe 합니다.

# 노드 실행 순서 
# 1. ROS 노드 이름 선언
# 2. Subscriber 생성
# 3. Callback 함수 생성 및 데이터 출력

#TODO: (3) Callback 함수 생성 및 데이터 출력
def callback(data):
    rospy.loginfo('\n my name : %s %s \n my age : %i \n SSAFY score : %i', data.first_name,data.last_name,data.age,data.score)

def listener():
    #TODO: (1) ROS 노드 이름 선언
    rospy.init_node('my_name_listener', anonymous=True)

    #TODO: (2) Subscriber 생성
    '''
    # student 라는 직접 만든 Custom ROS 메세지 형식을 사용하여 Topic Subscriber 를 완성한다.
    # Topic 이름은 'my_name' 으로 설정한다.
    rospy.Subscriber( 변수 1 , 변수 2 , callback)
    
    '''
    rospy.Subscriber("my_name", student, callback)  # 매우 중요!, 변수 1은 talker의 Pubkisher의 TOPIC 과 동일,2은 from ssafy_1.msg import student의 형태를 talker listener 모두 같은 형식으로 사용하고 있으므로 student. 
    rospy.spin()

if __name__ == '__main__':
    listener()

```


