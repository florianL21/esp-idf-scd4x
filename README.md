# Sensirion Embedded I2C SCD4x Driver ported to esp-idf component

This is a generic embedded driver for the [Sensirion SCD4x Carbon Dioxide Sensor](https://www.sensirion.com/scd4x/).
It enables developers to communicate with the SCD4x sensor on different hardware platforms by only adapting the I2C communication related source files.

[<center><img src="images/SCD4x.png" width="300px"></center>](https://sensirion.com/my-scd-ek)

# Getting started

## Add component to your esp-idf project

To add the component to your esp-idf based project, first clone this repository, preferably into your project directory but somewhere else should also work. 
```bash
git clone git://github.com/timbuntu/esp-idf-scd4x.git
```

Or add it as a submodule if you are also using git:
```
git submodule add git://github.com/timbuntu/esp-idf-scd4x.git
```

After cloning the repository, add the directory as extra component by adding the following line to the project cmake file, or append the directory to the line if it alrady exists (replace `esp-idf-scd4x` with the path to the cloned repo):

```cmake
set(EXTRA_COMPONENT_DIRS "esp-idf-scd4x")
```

Finally add the `esp-idf-scd4x` component to your own component by adding it as a requirement in the component `CMakeLists.txt` `idf_component_register` call so that it looks something like this:

```cmake
idf_component_register(SRCS "src/main.c" INCLUDE_DIRS "include/" REQUIRES "esp-idf-scd4x")
```

# Background

## Files

### sensirion\_i2c.[ch]

In these files you can find the implementation of the I2C protocol used by Sensirion
sensors. The functions in these files are used by the embedded driver to build the
correct frame out of data to be sent to the sensor or receive a frame of data from
the sensor and convert it back to data readable by your machine. The functions in
here calculate and check CRCs, reorder bytes for different byte orders and build the
correct formatted frame for your sensor.

### sensirion\_i2c\_hal.[ch]

In these files you can find the implementation of the hardware abstraction layer used
by Sensirion's I2C embedded drivers. This part of the code is specific to the underlying
hardware platform. This is an unimplemented template for the user to implement.
In the `sample-implementations/` folder we provide implementations for the most common
platforms.

### sensirion\_config.h

In this file we keep all the included libraries for our drivers and global defines.
Next to `sensirion_i2c_hal.c` *it's the only file you should need to edit to get your
driver working.*

### sensirion\_common.[ch]

In these files you can find some helper functions used by Sensirion's embedded drivers.
It mostly contains byte order conversions for different variable types. These functions
are also used by the UART embedded drivers therefore they are kept in their own file.
