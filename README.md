# Machine_learning_Altos

import cv2

cam = cv2.VideoCapture(0)
cv2.namedWindow("Press SPACE to capture, ESC to exit")
while True:
    ret, frame = cam.read()
    if not ret:
        print("Failed to grab frame")
        break

    cv2.imshow("Press SPACE to capture, ESC to exit", frame)

    k = cv2.waitKey(1)

    if k % 256 == 27:
        print("Escape hit, closing...")
        break
    elif k % 256 == 32:
        img_name = "captured_face.jpg"
        cv2.imwrite(img_name, frame)
        print(f" {img_name} saved!")
        break

cam.release()
cv2.destroyAllWindows()

import face_recognition
known_image = face_recognition.load_image_file("path_to_registered_face.jpg")
known_encoding = face_recognition.face_encodings(known_image)[0]

unknown_image = face_recognition.load_image_file("captured_face.jpg")
unknown_encoding = face_recognition.face_encodings(unknown_image)[0]

results = face_recognition.compare_faces([known_encoding], unknown_encoding)

if results[0]:
    print("Face Match! User is verified.")
else:
    print("Face Not Matching. Try again.")
