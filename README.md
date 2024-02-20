# Hand-Tracker
A short python code that can track hands along with fingers using opencv (cv2) and mediapipe

The project requires installation of the following Libraries
1) cv2
2) mediapipe
3) time

cv2 and mediapipe seems to be no longer supported on 3.12+ versions of python, cv2 especially was discontinued after 3.7
instead of cv2 opencv seems to be the proper one to be used

Note: installation was done as
pip install opencv-python
but it was imported into the code as
import cv2

remidiation attempts:
1) downgrading kernal to 3.7.4

    -> Problem with this is the ipykernel needs to be installed as ipynb format is the favourable format used by me.
    -> using terminal - pip install ipykernel
    -> https://stackoverflow.com/questions/64997553/python-requires-ipykernel-to-be-installed
   
3) downgrading kernal to 3.10.14 (working)
    -> Going to python website and downloading and installing 3.10.14
    -> Changing the kernal used inside VSCode






Legacy Code for handdetector  class:
class handDetector:
    def __init__ (
        self, mode=False, maxHands=2, mode1Comp=1, detectionCon=0.5, trackCon=0.5
    ):
        self.mode = mode
        self.maxhands = maxHands
        self.mode1Comp = mode1Comp
        self.detectionCon = detectionCon
        self.trackCon = trackCon
        self.mp_drawing = mp.solution.drawing_utils
        self.mp_drawing_styles = mp.solution.drawing_styles
        self.mp_hands = self.mp_hands.Hands(
            self.mode, self.maxHands, self.mode1Comp, self.detectionCon, self.trackCon #self.mp_hands = mp.solutions.hands.Hands(self.mode, self.maxHands, float(self.detectionCon), float(self.trackCon))
        )
    
    def findHands(self, image, draw = True):
        image.flags.writable = False
        image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
        self.results = self.hands.process(image)
        
        image.flags.writeable =True
        image = cv2.cvtColor(image, cv2.COLOR_BAYER_RGGB2BGR)
        if self.results.multi_hand_landmarks:
            for hand_landmarks in self.results.multi_hand_landmarks:
                #for id, lm in enumerate9hand_landmarks.landmark):
                #   print(id, lm)
                if draw:
                    self.mp_drawing.draw_landmarks(
                        image,
                        hand_landmarks,
                        self.mp_hands.HAND_CONNECTIONS,
                        self.mp_drawing_styles.DrawingSpec(color=(0,0,256)),
                        self.mp_drawing_styles.DrawingSpec(color=(0,256,0)),
                    )
            return image
        
    def finPos(self, image, handNo=0):
        lmList = []
        if self.results.multi_hand_landmarks:
            myHand = self.results.multi_hand_landmarks[handNo]
            
            for id, lm in enumerate(myHand.landmark):
                h,w,c = image.shape
                cx, cy = int(lm.x * w), int(lm.y *h)
                lmList.append([id, cx, cy])
        return lmList
    
    -------------------------------------------------------------------------------------------------------------

code 2:

import cv2
import mediapipe as mp

class handDetector:
    def __init__(self, mode=False, maxHands=2, detectionCon=0.5, trackCon=0.5):
        self.mode = mode
        self.maxHands = maxHands
        self.detectionCon = detectionCon
        self.trackCon = trackCon
        model_complexity=self.model_complexity
        self.mp_hands = mp.solutions.hands.Hands(self.mode, self.maxHands, min_detection_confidence=float(self.detectionCon), min_tracking_confidence=float(self.trackCon))

        #In the mediapipe library, the min_detection_confidence and min_tracking_confidence parameters should be specified as float values, but it seems there might be an issue with the way they are provided.
        #Make sure that self.detectionCon and self.trackCon are floating-point values. If they are not, you can convert them to floats explicitly:
        self.mp_drawing = mp.solutions.drawing_utils
        self.results = None

    def findHands(self, image, draw=True):
        imageRGB = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
        self.results = self.mp_hands.process(imageRGB)
        if self.results.multi_hand_landmarks:
            for handLms in self.results.multi_hand_landmarks:
                if draw:
                    self.mp_drawing.draw_landmarks(image, handLms, mp.solutions.hands.HAND_CONNECTIONS)
        return image

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
