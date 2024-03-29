# Table of Contents
- [1st Code Snippet](#1st-Code-Snippet)
- [2nd Code Snippet](#2nd-Code-Snippet)
- [3rd Code Snippet](#3rd-Code-Snippet)
- [4th Code Snippet](#4th-Code-Snippet)
- [5th Code Snippet](#5th-Code-Snippet)


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
*     class handDetector:
      def __init__(self, mode=False, maxHands=2, detectionCon=0.5, trackCon=0.5, model_complexity=1):  
        self.mode = mode  
        self.maxHands = maxHands  
        self.detectionCon = detectionCon  
        self.trackCon = trackCon  
        self.model_complexity = model_complexity
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
       self.mp_hands = mp.solutions.hands.Hands(
            self.mode, self.maxHands,
            model_complexity=self.model_complexity,
            min_detection_confidence=self.detectionCon,
            min_tracking_confidence=self.trackCon
        )
        self.mp_drawing = mp.solutions.drawing_utils
        self.results = None
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
     
## 3rd Code Snippet
## [ToC](#Table-of-Contents)
      def findHands(self, image, draw=True):
      imageRGB = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
        self.results = self.mp_hands.process(imageRGB)
        if self.results.multi_hand_landmarks and draw:
            for handLms in self.results.multi_hand_landmarks:
                self.mp_drawing.draw_landmarks(image, handLms, mp.solutions.hands.HAND_CONNECTIONS)
        return image

** We define a method named 'findHands' within the 'handDetector' class.
- def findHands(self, image, draw = True): Finds and draws landmarks on hands within the provided image
     - image: The input image (in BGR format) where hands are to be detected.
     - *BGR format (blue,gree,red) is the default color format in OpenCV*
     - draw (bool): A flag indicating whetehr to draw landmars on the image (default: True)
       
- imageRGB = cv2.cvtColor(image, cv2.COLOR_BGR2RGB): converts the BGR image to RGB as the mediapipe Hands model expects RGB input ( if image is already in RGM format this is not needed)

- self.results = self.mp_hands.process(imageRGB): Process the image using the mdeiapipe Hands model, reults are stored in self.results.

- if self.reults.multi_hand_landmars and draw: Checks if there are multiple hand landmarks and drawing is enabled, the landmarks and connections between them are drawn on the image.

- for handLms in self.results.multi_hand_landmarks: Iterates through each set of landmarks for a different hands.

- self.mp_drawing.draw_landmarks(image, handLms, mp.solutions.hands.HAND_CONNECTIONS): Draw landmarks and connections on the image

- return image: Returns the image with hand landmarks drawn (if draw is True)

## 4th Code Snippet
## [Toc](#Table-of-Contents)
        def findPos(self, image, handNumber=0, draw=True):
              lmList = []
              if self.results.multi_hand_landmarks:
                  myHand = self.results.multi_hand_landmarks[handNumber]
                  for id, lm in enumerate(myHand.landmark):
                      h, w, c = image.shape
                      cx, cy = int(lm.x * w), int(lm.y * h)
                      lmList.append([id, cx, cy])
                      if draw:
                          cv2.circle(image, (cx, cy), 15, (255, 0, 255), cv2.FILLED)
              return lmList

**We define a method named findPos within the handDetector class. This method is designed to extract and return the positions(coordinates) of specific landmarks on a detected hand.**

- **def findPos(self image, handNumber=0, draw = True)**: Finds the position (coordinates) of landmarks on a detected hand.
        - **image**: The input image (in BGR format) containing the detected hand.
        - **handNumber (int)**: The index of the hand to extract landmarks from. (default: 0)
        - **draw (bool)**: A flag indicating whether ot draw cricles at landmark positions.( default: True )
  
- **lmList**: a list containing the landmark positions [ id, cx, cy] for the specified hand.
  
- **if self.results.multi_hand_landmarks**: Checks if there are multiple hand landmarks in the results.
  
- **myHand = self.results.multi_hand_landmarks[handNumber]**: Extract landmarks for the specified handNumber.
  
- **for id, lm in enumerate(myHand.landmark)**: iterate through each landmark in the specified hand and calculates the pixel coordinates(cx and cy) based on the image dimensions
  
- **h,w,c = image.shape:**
        - **image.shape**: returns the tuple representing the dimensions of the image.
        - 'h'(height), 'w'(width), 'c'(number of channels) are assigned values from the tuples.
        - commonly used to get height, width and number of channels of the image.
  
- **cx,cy = int(lm.x * w), int(lm.y * h)**
        - **lm**: landmark object with x and y attributes representing normalized coordinates in the range[0,1]
        - **lm.x * w**: calculates the pixel position in the horizontal (x) direction.
        - **lm.y * h**: calculates the pixel position in the verical (y) direction.
        - **int()**: is used to convert the floating-point values to integers, as pixel position are whole numbers
        - The line calculates the pixel coordinates of a landmark based on its normalized coordinates(lm.x and lm.y) and the dimensions of the image('w' and 'h'). The resulting 'cx' and 'cy' represent the position of the landmark in pixel units.
  
- **lmList.append([id, cx, cy]):** Append the landmark id and coordinates to lmList
  
- **if draw: cv2.cricle( image, (cx, cy), 15, (255,0,255), cv2.FILLED)**: Draw a filled circle at the landmark position if draw is True
  
- **return lmList**: reutrns the list of landmark positions

  ## 5th Code Snippet
  ## [ToC](#Table-of-Contents)

        def main():
          pTime = 0
          cTime = 0
          cap = cv2.VideoCapture(0)
          detector = handDetector()
          while True:
              success, image = cap.read()
              image = detector.findHands(image)
              cTime = time.time()
              fps = 1 / (cTime - pTime)
              pTime = cTime
              image = cv2.flip(image, 1)
              cv2.putText(
                  image,
                  str(int(fps)),
                  (500,400),
              cv2.FONT_HERSHEY_PLAIN,
              3,
              (120, 52, 40),
              3,
              )
              cv2.imshow("Camera result", image)
              landmarks = detector.findPos(image)
              if len(landmarks) != 0:
                  print(landmarks[4])
              cv2.waitKey(1)
              
      if __name__ == "__main__":
          main()

- **detector = handDetector()**: Intitializes the handDetector object

- **while True**: This is the main loop for capturing frames and handtracking, we initialize it as an infinite loop to continuously capture frames from the webcam while performing hand tracking.

- **success, image = cap.read()**: captures a frame from the webcam using the 'cap.read()' method. 'success' indicates whether the frame was suceesfully captured, and 'image' contains the captured frame.

- **image = detector.findHands(image)**: we process the frame to find hand landmarks using handDetector instance. we call the 'findHands' method of the 'handDetector' instance to detect and annottate hand landmarks on the captured frame.

- **cTime = time.time()** : here time.time() returns the current time in seconds since the epoch( reference point in time). Here it is used to get the current time('ctime') when processing the current frame.

- **fps = 1/ (cTime - pTime)**: Computes the reciprocal of the time taken to process the current frame('cTime - pTime'). the result is assigned to the variable 'fps'.

- **pTime = cTime**: updates the previous time ('ptime') to be equal to the current time, ensuring the next iteration of the loop, the previous time represents the time at the start of processing the next frame.

- **cv2.putTex()**: adds a text overlay to the image displaying the calculated frames per second.

- **cv2.imshow("Camera result", image)**: displays the annotated image in a window titled " Camera result."

- **landmarks = detector.findPos(image)**: find and print the position of a specific landmark

- **if len(landmarks) != 0 **: 

- **print(landmarks[4])**: Assuming landmarks[4] represent the tip of the index finger.

- We call the 'findPos' method of the 'handDetector' instance to find the positions of hand landmarks. If landmarks are found, it prints the position of a specific landmark ( assuming landmraks[4] represents the tip of the index finger).

- **cv2.waitkey(1)**: Waits for a key press with a dely of a millisecond. This delay allows the OpenCV window to handle events such as window closing.

- pTime and cTime are variables used to calculate and display the frames per second(fps)

- **cv2.VideoCapture(0)**: initializes webcam capture.

- **handDetector()**: creates an instance of the 'handDetector' class for hand tracking.

- Inside the loop, each frame is read from the webcam, prcoessed to find hand landmarks and then displayed.

- The fps is calculated and displayed on the frame.

- The image is horizontally flipped for better user experience.

- The position of a specific landmark( eg. the tip of index finger) is printed in the console.

- The loop continues until a key is pressed and the program exits when the user closes the window.

## [ToC](#Table-of-Contents)
