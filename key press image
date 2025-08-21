from keras.models import load_model
import numpy as np

import cv2 
from picamera2 import Picamera2
from libcamera import Transform
import time

np.set_printoptions(suppress=True)
model = load_model("keras_modelv3.h5", compile=False)
class_names = open("labelsv3.txt", "r").readlines()

picam2 = Picamera2()
camera_config = picam2.create_still_configuration(main={"size": (1640,1232)},transform=Transform(vflip=True,hflip=True))
picam2.configure(camera_config)
picam2.start()

capture_time = None

while True:
    frame = picam2.capture_array()
    frame = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
    
    key = cv2.waitKey(1) & 0xFF
    if key == ord('c'):
        image = cv2.resize(frame, (224, 224), interpolation=cv2.INTER_AREA)
        image = np.asarray(image, dtype=np.float32).reshape(1, 224, 224, 3)
        image = (image / 127.5) - 1

        prediction = model.predict(image)
        index = np.argmax(prediction)
        class_name = class_names[index]
        confidence_score = prediction[0][index]
        result = class_name[2:-1]
        print(result)
      
        capture_time = time.time()
            
    elif key == ord('q'):
        break
    
    if capture_time is not None and time.time() - capture_time < 1:
        cv2.putText(frame, result, (50, 50), cv2.FONT_HERSHEY_SIMPLEX, 2, (0, 255, 0), 3, cv2.LINE_AA)
        
    cv2.imshow("Video Stream", frame)
     
picam2.stop()
cv2.destroyAllWindows()
