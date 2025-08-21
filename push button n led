from gpiozero import LED
from time import sleep
from gpiozero import Button

button = Button(2)
led = LED(23)

button.wait_for_press()
print("You pushed me!")
print("Lighting Up!")
led.on()
sleep(3)
led.off()
