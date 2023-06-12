# facedrtect-cam2
import cv2

cap = cv2.VideoCapture(0)
cap.set(cv2.CAP_PROP_FRAME_WIDTH,320)
cap.set(cv2.CAP_PROP_FRAME_HEIGHT,240)

if not cap.isOpened():
    print("cap open failed")
    exit()

face_xml = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_frontalface_default.xml')

face_li = []
mosaic_loc = []

while True:
    ret, img = cap.read()
    if not ret:
        print("Can't read cap")
        break

    img_gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

    faces = face_xml.detectMultiScale(img_gray, 1.3, 5) #얼굴인식
    face_li.append(faces)
    
    if len(faces) >= 3:
        # x, y, w, h = faces[0][0]
        for k in range(3):
            face_li[k] = faces[k][0],faces[k][1],faces[k][2],faces[k][3] #face_li = 0이면 x1,y1,w1,h1이다.

        for a in range(3):
            #얼굴 부분 자르기
            mosaic_loc.append(img[faces[a][1]:faces[a][1]+faces[a][3], faces[a][0]:faces[a][0]+faces[a][2]])
        
        #0번째 1번째 이미지 변경
        for i in range(3):
            mosaic_loc[i] = cv2.resize(mosaic_loc[i], (faces[i+1][2], faces[i+1][3]), cv2.INTER_LINEAR)
            if i == 2:
                mosaic_loc[2] = cv2.resize(mosiac_loc[i] (faces[0][2], faces[0][3]), cv2.INTER_LINEAR)

        for j in range(3):
            img_w_mosaic[faces[j][1]:faces[j][1]+faces[j][3], faces[j][0]:faces[j][0]+faces[j][2]] = mosaic_loc[j+1]
            if j == 2:
                img_w_mosaic[faces[j][1]:faces[j][1]+faces[j][3], faces[j][0]:faces[j][0]+faces[j][2]] = mosaic_loc[0]

    cv2.imshow("Face Recognition", img)

    if cv2.waitKey(1) == ord('q'):
        break

cv2.destroyAllWindows()
