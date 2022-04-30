# opencv 查找正方形

```python
def findRect():
    import cv2
    import numpy as np

    # img = cv2.pyrDown(cv2.imread("titlename.png", cv2.IMREAD_UNCHANGED))
    img = cv2.imread("titlename.png", cv2.IMREAD_UNCHANGED)
    ret, thresh = cv2.threshold(cv2.cvtColor(img.copy(), cv2.COLOR_BGR2GRAY), 127, 255, cv2.THRESH_BINARY)
    contours, hier = cv2.findContours(thresh, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    # python3 3个返回值
    # _, contours, hier = cv2.findContours(thresh, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    for c in contours:
        # find bounding box coordinates
        x, y, w, h = cv2.boundingRect(c)
        if 360 != w and h != 96:
            continue
        else:
            print x, y, w, h
        cv2.rectangle(img, (x, y), (x + w, y + h), (0, 255, 0), 2)

        # find minimum area
        rect = cv2.minAreaRect(c)
        # calculate coordinates of the minimum area rectangle
        box = cv2.boxPoints(rect)

        # normalize coordinates to integers
        box = np.int0(box)
        print box
        # draw contours
        cv2.drawContours(img, [box], 0, (0, 0, 255), 3)

        # calculate center and radius of minimum enclosing circle
        # (x, y), radius = cv2.minEnclosingCircle(c)
        # cast to integers
        # center = (int(x), int(y))
        # radius = int(radius)
        # draw the circle
        # img = cv2.circle(img, center, radius, (0, 255, 0), 2)

    cv2.drawContours(img, contours, -1, (255, 0, 0), 1)
    cv2.imshow("contours ", img)

    cv2.waitKey()
    cv2.destroyAllWindows()

```

---

> [跳转到目录](menu.md)
