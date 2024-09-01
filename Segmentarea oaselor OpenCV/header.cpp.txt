#pragma once
#include <iostream>
#include "opencv2/core.hpp"
#include "opencv2/highgui.hpp"
#include "opencv2/imgproc.hpp"
#include <opencv2/opencv.hpp>

using namespace cv;


Mat enhance(Mat input_image);
void erode_and_dilate(Mat& input_image, int iterations);
Mat iterative_threshold(Mat input_image);
Mat colour_segmentation(Mat input_image);
void fill_enclosed_spaces(Mat& input_image);
