# attiny-i2c-master

An implementation of I2C master for Attiny microcontrollers, using USI.

This is useful for projects with multiple MCUs that are already implementing an I2C bus, to offload some logic from the main MCU and/or expand ports.


## How it works?

This library uses the Universal Serial Interface (USI), because most of the Attinys don't implement Two Wire Interface (TWI).

TODO: expand


## How to use it?

Just copy `i2c-master.h` and `i2c-master.c` to your project and add them to your build system. `test.c` is an usage example. The [API](#api) section below explains the usage of the functions exported by the library.


## API

The header `i2c-master.h` exports 3 functions:


### `i2c_master_init`

```c
void i2c_master_init();
```

This function initializes the USI stack (including ports).


### `i2c_master_write_data`

```c
bool i2c_master_write_data(uint8_t slave_address, uint8_t reg_address, uint8_t *data, size_t data_len);
```

This function writes to an I2C device connected to the bus.

The arguments of this function are:

- `slave_address`: Address of the slave that should receive the data.
- `reg_address`: Address of the register of the slave that should receive the data.
- `data`: Array with the data to be written.
- `data_len`: Length of the data array.

This function returns `true` if the data was correctly written, otherwise `false`.


### `i2c_master_read_data`

```c
bool i2c_master_read_data(uint8_t slave_address, uint8_t reg_address, uint8_t *data, size_t data_len);
```

This function reads from an I2C device connected to the bus.

The arguments of this function are:

- `slave_address`: Address of the slave that should send the data.
- `reg_address`: Address of the register of the slave that should send the data.
- `data`: Array to store the read data.
- `data_len`: Length of the data array.

This function returns `true` if the data was correctly read, otherwise `false`.


## License

This library is released under a 3-clause BSD license. See `LICENSE` file for details.
