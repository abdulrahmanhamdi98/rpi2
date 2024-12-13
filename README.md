import RPi.GPIO as GPIO
import time

GPIO.setwarnings(False)
GPIO.cleanup()
GPIO.setmode(GPIO.BCM)

# GPIO pin assignments
TRIG = 14
ECHO = 18
GREEN = 17
YELLOW = 27
RED = 22
BUZZER = 23

# GPIO setup
GPIO.setup(TRIG, GPIO.OUT)
GPIO.setup(ECHO, GPIO.IN)
GPIO.setup(GREEN, GPIO.OUT)
GPIO.setup(YELLOW, GPIO.OUT)
GPIO.setup(RED, GPIO.OUT)
GPIO.setup(BUZZER, GPIO.OUT)

def green_light():
    GPIO.output(GREEN, GPIO.HIGH)
    GPIO.output(YELLOW, GPIO.LOW)
    GPIO.output(RED, GPIO.LOW)
    GPIO.output(BUZZER, GPIO.LOW)

def yellow_light():
    GPIO.output(GREEN, GPIO.LOW)
    GPIO.output(YELLOW, GPIO.HIGH)
    GPIO.output(RED, GPIO.LOW)
    GPIO.output(BUZZER, GPIO.LOW)

def red_light():
    GPIO.output(GREEN, GPIO.LOW)
    GPIO.output(YELLOW, GPIO.LOW)
    GPIO.output(RED, GPIO.HIGH)
    GPIO.output(BUZZER, GPIO.HIGH)

def get_distance():
    GPIO.output(TRIG, True)
    time.sleep(0.00001)
    GPIO.output(TRIG, False)

    while GPIO.input(ECHO) == False:
        start = time.time()

    while GPIO.input(ECHO) == True:
        end = time.time()

    signal_time = end - start
    distance = signal_time / 0.000058  # Calculate distance in cm
    return distance

try:
    while True:
        distance = get_distance()
        time.sleep(0.05)  # Small delay to stabilize readings

        print(f"Distance: {distance:.2f} cm")

        if distance >= 25:
            green_light()
        elif 25 > distance > 10:
            yellow_light()
        elif distance <= 10:
            red_light()

except KeyboardInterrupt:
    print("Exiting program.")
    GPIO.cleanup()
