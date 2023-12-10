import cv2
import numpy as np

file_path1 = "C:\\Users\\20629\\file\\stream.mp4"
vedio = cv2.VideoCapture(file_path1)

while vedio.isOpened():

    ret, frame = vedio.read()

    if not ret:

        break

    gray_image = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    _, binary_image = cv2.threshold(gray_image, 125, 255, cv2.THRESH_BINARY)
    contours, _ = cv2.findContours(binary_image, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    for cnt in contours:
        area = cv2.contourArea(cnt)
        if  area < 300:
            x, y, w, h = cv2.boundingRect(cnt)
            cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)
    for cnt in contours:
        rect = cv2.minAreaRect(cnt)
        angle = rect[2]
        print("Object angle: {:.2f} degrees".format(angle))
    cv2.imshow('video',frame)
    if cv2.waitKey(25) & 0xFF == ord('q'):
        break
print()
vedio.release()
cv2.destroyAllWindows()
