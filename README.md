# Galory
Some temparary shared code
 Inpaint depth map.
 https://www.youtube.com/watch?v=u0A4OVZxzKQ

Mat depthMat(height, width, CV_16UC1, depth); // from kinect
Mat depthf(height, width, CV_8UC1);

depthMat.convertTo(depthf, CV_8UC1, 255.0/2048.0);
imshow("original-depth", depthf);

const unsigned char noDepth = 0; // change to 255, if values no depth uses max value
Mat temp, temp2;

// 1 step - downsize for performance, use a smaller version of depth image
Mat small_depthf;
resize(depthf, small_depthf, Size(), 0.2, 0.2);

// 2 step - inpaint only the masked "unknown" pixels
cv::inpaint(small_depthf, (small_depthf == noDepth), temp, 5.0, INPAINT_TELEA);

// 3 step - upscale to original size and replace inpainted regions in original depth image
resize(temp, temp2, depthf.size());
temp2.copyTo(depthf, (depthf == noDepth)); // add to the original signal

imshow("depth-inpaint", depthf); // show results

Original code adapted from:
http://www.morethantechnical.com/2011...
