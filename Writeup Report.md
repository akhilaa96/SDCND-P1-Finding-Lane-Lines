
(a) Steps in Pipeline:
Process one image, detect two lanes and highlight them with solid color lines
    (1) apply the gaussian blur
    (2) convert bgr to hsv and segment while and yellow, because it is easier in HSV space than RGB
    (3) Canny edge detection
    (4) apply the designed mask to the image to obtian the region of interest
    (5) apply hough transform to get lines
    (6) augmented the lanes on the original image

(b)How to handle the shadow area in "challenge.mp4". In test images, it was in RGB space instead of the HSV space, and that version works quite OK until meet the “challenge.mp4”. It starts to fail when the car drives into the shadow of the trees, in which case edge detector tends to grab the boundaries of the shadow instead of the yellow lane under the shadow. After I switch to HSV space, the lane extraction is much more robust.

(c) How to get a stable lane: Firstly, I buffer the left and right lanes' slope and intercepts of N frames, then when I average the current frame's slope and intercept with the previous one. Secondly, when I draw a lane with computed slope and intercept, I fix the top point's vertical position. Thirdly, I try to remove the outliers by discarding some horizontal lines. Fourthly, within the linear regression, I remove 10% of points that have the largest residual errors. Due to there are not many outliers, I only iterate the outlier removal once.

(d) How to make it better: Firstly, since I fix two lanes' top point's vertical location, so when it comes to sharp curved road, the lane will overshoot. I consider to use better fit than linear line to get better visual appealing. Secondly, if we want to make it more visual appealing, we can segment all visible lanes in the road, and highlights the with different colors, e.g. the both the driving lane with green, and the other lane for the coming cars with red.
