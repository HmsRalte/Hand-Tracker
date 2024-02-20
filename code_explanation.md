## Firstly, we have to install the libraries that are going to be needed
- pip install opencv-python
- pip install mediapipe
- pip install times

### Importing the libraries comes next
- import cv2
- import mediapipe as mp
- import times

## 1st code snippet  
class handDetector:
  def __init__(self, mode=False, maxHands=2, detectionCon=0.5, trackCon=0.5, model_complexity=1):  
    self.mode = mode  
    self.maxHands = maxHands  
    self.detectionCon = detectionCon  
    self.trackCon = trackCon  
    self.model_complexity = model_complexity  
        
**we intialize an instance of the handDetector class with specified parameters.**
- mode(bool): Flag, indicating the mode of the hand detection (default: False)
- maxHands(int): Maximum number of hands to be detected(default: 2)
- detectionCon (float): Confidence threshold for hand detection (default: 0.5).
- trackCon (float): Confidence threshold for hand tracking (default : 0.5).
- mcdel_complexity (int): Model complexity for hand deteection (default: 1)

**The __init__ mothod is the constructor for the handDetector class. It initializes the instance variables with default or user-provided values.**
**NOTE: in Python 'self; is a convention used as the first parameter in the method definitions of a class. It represents the instance of the class allowing the access and manipulation of the instances attributes and methods. Although any other name is usable, it is higly recommended to use self to enchance code readability and maintainability**
