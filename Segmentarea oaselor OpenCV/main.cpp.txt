#include "Header.h"
#include <vector>
#include <string>

int main()
{
	// Vector de imagini
	std::vector<std::string> boneImages = { "bone1", "bone2", "bone0", "bone3", "bone4", "bone5", "bone6", "bone7", "bone8"};
	

	for (const auto& boneImage : boneImages)
	{
		//	IMREAD_GRAYSCALE converteste imaginea in grayscale
		Mat source_img = imread("img/" + boneImage + ".png", IMREAD_GRAYSCALE);
		Mat sharpened_img, iterative_threshold_img, colour_threshold_img;

		if (source_img.empty())
		{
			std::cerr << "Could not open or find the image: " << "img/" + boneImage + ".png" << std::endl;
			continue;
		}

		imshow("Original", source_img);


		// accentual sharpness-ul si contrastul imaginii pentru a delimita mai clar zonele
		sharpened_img = enhance(source_img);
		imshow("Imagine enhanced", sharpened_img);


		// Segmentare prin aplicarea unui threshold iterativ
		iterative_threshold_img = iterative_threshold(sharpened_img);
		imshow("Threshold iterativ", iterative_threshold_img);


		// metoda de erodare + dilatare pentru a reduce zgomotul din segmentari
		erode_and_dilate(iterative_threshold_img, 3);
		imshow("Dilate & erode", iterative_threshold_img);



		// Crearea unei copii a imaginii erodate si dilatate pentru a combina cu imaginea cu spatiile inchise
		Mat eroded_dilated_img = iterative_threshold_img.clone();

		// Umplem spatiile inchise din imagine 
		fill_enclosed_spaces(iterative_threshold_img);
		imshow("Filled Enclosed Spaces", iterative_threshold_img);

		// Combinam imaginea cu spatiile inchise cu imaginea erodata si dilatata
		bitwise_or(eroded_dilated_img, iterative_threshold_img, iterative_threshold_img);
		imshow("Combined Image with Filled Holes", iterative_threshold_img);

		// Facem imaginea binara 
		threshold(iterative_threshold_img, iterative_threshold_img, 128, 255, THRESH_BINARY);

		colour_threshold_img = colour_segmentation(iterative_threshold_img);
		imshow("Coloured img", colour_threshold_img);

		waitKey(0);
		destroyAllWindows();
	}

	return 0;
}

