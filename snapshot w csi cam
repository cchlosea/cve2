import cv2
from picamera2 import Picamera2
from datetime import datetime
from libcamera import Transform
import time

picam2 = Picamera2()
    
camera_config = picam2.create_still_configuration(main={"size": (1640,1232)},transform=Transform(vflip=True,hflip=True))
picam2.configure(camera_config)
picam2.start()

capture_time = None

while True:
    frame = picam2.capture_array()
    frame = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
        
    key = cv2.waitKey(1) & 0xFF
    
    if key == ord('q'):
        break
    
    elif key == ord('c'):
        filename = datetime.now().strftime("image_%Y%m%d_%H%M%S.png")
        cv2.imwrite(filename, frame)
        text = f"Captured {filename}"
        print(text)
        capture_time = time.time()
        
    if capture_time is not None and time.time() - capture_time < 0.5:  
        cv2.putText(frame, f"Captured {filename}", (50, 50), cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 255, 0), 3, cv2.LINE_AA)
        
    cv2.imshow("Video Stream", frame)
    

picam2.stop()
cv2.destroyAllWindows()
