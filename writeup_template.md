## Reflection

### 1. Pipeline description. As part of the description, I explain how I modified the draw_lines() function.

My pipeline consist of 10 steps:
1. Copy original image 
    - This step is necessary to not modify the input
2. Get image size
    - We need the geometry of the image for the following steps
3. Filter white and yellow lines
    - This step is necessary to remove mirror effect from the hood in the additional challenge
4. Conver to grayscale
    - A typical step in image processing to remove some noise, reduce the image size, the simpler structure of a grayscaled image
5. Blur image for the Canny operator
    - To increase the efficiency of Canny operator
6. Run canny over the image
    - This step should find edges on the image
7. Define a region of interest
    - Here we defined a zone in front of the car as our region of interest in which we expect to see lines
8. Select region - trapezoid
    - Black the region outside of our region of interest
9. Extract lines with Hough function
    - Find lines on the image
10. Draw lines on image and return result
    - Average and draw the lines on the image 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by:
1. Checking the slope of the line, which defines with a high probability to which sideline belongs.
2. Calculate the middle of the line
2. In order to remove outliers, I check that center of the "left" line is on the left part of the image, same for "right", if not I ignore this line, else I add them to appropriate list.
3. Then for "left" and "right" list I use **np.polyfit** function to build approximation.
4. Last step: using the **np.poly1d**  function I calculate the beginning and end of the lines and draw them.

### 2. Potential shortcomings with current pipeline
From my point of view, few potential shortcomings are:
1. The pipeline is sensitive to the quality of the lines color and line visibility. Different line color, rain or snow can cause recognition issues.
2. The pipeline is sensitive to the sharp turns
3. White or yellow car in front can cause the problem as well

### 3. Possible improvements to pipeline
Assuming the current implementation and used approach, I see two major things to improve:
   - Select coefficients and color boundaries more precisely
   - Improve approximation in **draw_lines()** function, the way how outlier is removed currently is not elegant