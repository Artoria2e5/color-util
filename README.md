# color-util
A header-only C++11 library for handling colors, including color space converters between RGB, XYZ, Lab, etc. and color difference calculators such as CIEDE2000.

## Color Space Convertors

### RGB-to-HSL

An RGB color can be converted to an HSL color using by `convert_RGB_to_HSL`. This function is defined in `color-util/RGB_to_HSL.hpp`.

### RGB-to-XYZ

A sRGB color can be converted to a CIEXYZ color using by `convert_RGB_to_XYZ`. This function is defined in `color-util/RGB_to_XYZ.hpp`.

### XYZ-to-Lab

A CIEXYZ color can be converted to a CIELAB (a.k.a. CIE L\*a\*b\*) color using by `convert_XYZ_to_Lab`. This function is defined in `color-util/XYZ_to_Lab.hpp`.

## Color Difference Calculators

### CIEDE2000

Given two CIELAB colors, `calculate_CIEDE2000` calculates a *perceptual* difference between these two colors based on a metric called CIEDE2000. This function is defined in `color-util/CIEDE2000.hpp`. 

The correctness of the implementation is verified through the test dataset provided by Gaurav Sharma <http://www2.ece.rochester.edu/~gsharma/ciede2000/>.

## Dependency

- Eigen <http://eigen.tuxfamily.org/>

## Installation

color-util is a *header-only* library, so it can be used by just copying the directory named `color-util` to the `include` directory of the target project. No need to build it in advance.

For CMake <https://cmake.org/> users, this repository includes `CMakeLists.txt`; it can be installed by 
```
cmake [PATH_TO_THIS_REPOSITORY] -DCMAKE_INSTALL_PREFIX=[PATH_TO_INSTALL_DIRECTORY]
make install
```
Alternatively, if the target project is also managed by CMake, the `ExternalProject_Add` command is also useful.

## Example

Here is an example code for calculating a perceptual distance of two sRGB colors. First, include necessary headers:
```
#include <color-util/RGB_to_XYZ.hpp>
#include <color-util/XYZ_to_Lab.hpp>
#include <color-util/CIEDE2000.hpp>
```
Define the target colors in sRGB:
```
colorutil::RGB rgb_color_1(200.0 / 255.0, 100.0 / 255.0, 20.0 / 200.0);
colorutil::RGB rgb_color_2(100.0 / 255.0, 200.0 / 255.0, 50.0 / 200.0);
```
CIEDE2000 requires CIELAB colors as input, so convert them to CIELAB via CIEXYZ:
```
colorutil::XYZ xyz_color_1 = colorutil::convert_RGB_to_XYZ(rgb_color_1);
colorutil::XYZ xyz_color_2 = colorutil::convert_RGB_to_XYZ(rgb_color_2);
colorutil::Lab lab_color_1 = colorutil::convert_XYZ_to_Lab(xyz_color_1);
colorutil::Lab lab_color_2 = colorutil::convert_XYZ_to_Lab(xyz_color_2);
```
Finally, calculate the perceptual distance of the two colors:
```
double difference = colorutil::calculate_CIEDE2000(lab_color_1, lab_color_2);
```
In this case, it returns `53.8646` as the perceptual distance between the two colors.

## License

MIT License.

## Contribute

Pull requests are welcome.
