# Hand Pose Detection With Unreal

## What I learn from this project?
### 1. Enum class
- HandLandmark is Enum class in Mediapipe. Iterable.
```python
HandLandmark = mediapipe.python.solutions.hands 
for name in HandLandmark:
    name = str(name).split('.')[-1]
    NAME_LIST.append(name)
```


### 2. Cheating for performance.
- instead of flipping image in cv, just flipping left and right.
```python
def write_json_file(left_right:str, pose_dict):
    # alternatives of flipping image
    if left_right == "Left":
        left_right = "Right"
    elif left_right == "Right":
        left_right = "Left"
```


### 3. Unreal Coordinate System VS MP Coordinate System
- Unreal use X+ Forward. Y+ Right, Z+ Up.
- Mediapipe use different one. check it on [url](https://google.github.io/mediapipe/solutions/objectron.html#coordinate-systems)
```python
pose_dict = {}
for i in range(len(hand_landmarks.landmark)):
    # Unreal Coordinate System : X Forward, Z Up
    # MP Coordinate System     : https://google.github.io/mediapipe/solutions/objectron.html#coordinate-systems
    pose_dict[NAME_LIST[i]] = {'X': hand_landmarks.landmark[i].z , 'Y': hand_landmarks.landmark[i].x , 'Z':hand_landmarks.landmark[i].y * -1}
```

### 4. Threading
- running python script on unreal engine pause unreal sometimes. thread will solve this
```python
global_thread = threading.Thread(target=detect_hand)
global_thread.start()
```
- In my case  Communication goes through Json file. So, running python on powershell instead of unreal editor didn't require threading. but run python script on unreal is useful!


### 5. Implementing Gesture
![img](imgs\Five.png)

 The hardest problem during this project is implementing gesture functionality.
Distance between Camera and Hands make so many diversity in value.
So, Measuring Offset value and applying it on detecting what this gesture is is complicated and not accurate.
 
 I feel I have to learn Graphics Libarary and Inverse Kinematics. Applying Nerual Network works great on this part either. I consider it either.

The Gesture I called "Five" and Hand up is the most reliable and easy to implement. Just check every Z value with lots of combination you have to check.

 By using this, Player could pass 4-different gesture input. left up and right up. only left up, onlyt right up, none. 


### Reference
- https://google.github.io/mediapipe/solutions/hands.html#python-solution-api, Original Code
- https://typecast.ai/en, text to speech solution.
- https://www.youtube.com/watch?v=MVIykxtV0Wo, Blinker Sound
- https://google.github.io/mediapipe/, Pose Detection
- https://docs.unrealengine.com/4.27/en-US/- InteractiveExperiences/Vehicles/, Vehicle Project Reference
- https://microsoft.github.io/AirSim/, Audio Vehicle, Engine Sound.