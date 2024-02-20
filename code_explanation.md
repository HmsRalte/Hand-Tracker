# Table of Contents
- [1st Code Snippet](#1st-Code-Snippet)
- [2nd Code Snippet](#2nd-Code-Snippet)


## Firstly, we have to install the libraries that are going to be needed
- pip install opencv-python
- pip install mediapipe
- pip install times

### Importing the libraries comes next
- import cv2
- import mediapipe as mp
- import times

## 1st Code Snippet 
## [ToC](#Table-of-Contents)
* class handDetector:
  * def __init__(self, mode=False, maxHands=2, detectionCon=0.5, trackCon=0.5, model_complexity=1):  
    * self.mode = mode  
    * self.maxHands = maxHands  
    * self.detectionCon = detectionCon  
    * self.trackCon = trackCon  
    * self.model_complexity = model_complexity
---
        
**We intialize an instance of the handDetector class with specified parameters.**
- *mode(bool)*: Flag, indicating the mode of the hand detection (default: False)
- *maxHands(int)*: Maximum number of hands to be detected(default: 2)
- *detectionCon (float)*: Confidence threshold for hand detection (default: 0.5).
- *trackCon (float)*: Confidence threshold for hand tracking (default : 0.5).
- *mcdel_complexity (int)*: Model complexity for hand deteection (default: 1)

**The __init__ mothod is the constructor for the handDetector class. It initializes the instance variables with default or user-provided values.**

**NOTE: in Python 'self; is a convention used as the first parameter in the method definitions of a class. It represents the instance of the class allowing the access and manipulation of the instances attributes and methods. Although any other name is usable, it is higly recommended to use self to enchance code readability and maintainability**

## 2nd Code Snippet
## [ToC](#Table-of-Contents)
* self.mp_hands = mp.solutions.hands.Hands(
  * self.mode, self.maxHands,
  * model_complexity=self.model_complexity,
  * min_detection_confidence=self.detectionCon,
  * min_tracking_confidence=self.trackCon
    * )
    * self.mp_drawing = mp.solutions.drawing_utils
    * self.results = None
      
---
  
**We initialize the mediapipe Hands object for hand detection**

  - *self.mp_hands*: intializes a mediapipe hands object with specified parameters
  - *mp.solutions.hands.Hands*: refers to 'Hands' class within the 'hands' module of mediapipe library.
     - *mp*: reference to mediapipe at the moment of importing the library
     - *solutions*: submodule within the mediapipe library containing pre-built solutions or models for various computer vision tasks.
     - *hands*: wihtin 'soultuions' submodule, ;hands; specifically refers to the module responsible for hand tracking
     - *Hands*: This is the 'Hands' ca=lass within the ;hands; module. It is likely the main class for hand tracking in mediapipe and is used ofr initializing an instance of the hand tracking model.
   - *self.mp_drawing*: initializes the mdeiapipe drawing utilities module
   - *self.results*: initializes a variable to store the results of hand detection. It is initially set to 'None'.
  

