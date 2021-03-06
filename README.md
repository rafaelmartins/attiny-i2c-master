# attiny-i2c-master

An implementation of I2C master for Attiny microcontrollers, using USI.

This is useful for projects with multiple MCUs that are already implementing an I2C bus, to offload some logic from the main MCU and/or expand ports.


## How it works?

This library uses the Universal Serial Interface (USI), because most of the Attinys don't implement Two Wire Interface (TWI).

TODO: expand


## How to use it?

Just copy `i2c-master.h` and `i2c-master.c` to your project and add them to your build system. `test.c` is an usage example. The [API](#api) section below explains the usage of the functions exported by the library.


## API

The header `i2c-master.h` exports several functions. The final API available for usage depends on a macro definition:

- If `I2C_LOW_LEVEL_API` is defined, only low level functions will be available.
- If `I2C_LOW_LEVEL_API` is not defined (default), only high level functions will be available.

The initialization function `i2c_master_init` is always available.


### `i2c_master_init`

```c
void i2c_master_init();
```

This function initializes the USI stack (including ports).


## API - High level (default)

These functions are defined by default, when `I2C_LOW_LEVEL_API` is not defined.


### `i2c_master_write`

```c
bool i2c_master_write(uint8_t slave_address, uint8_t reg_address, uint8_t *data, size_t data_len);
```

This function writes to an I2C device connected to the bus.

The arguments of this function are:

- `slave_address`: Address of the slave that should receive the data.
- `reg_address`: Address of the register of the slave that should receive the data.
- `data`: Array with the data to be written.
- `data_len`: Length of the data array.

This function returns `true` if the data was correctly written, otherwise `false`.


### `i2c_master_read`

```c
bool i2c_master_read(uint8_t slave_address, uint8_t reg_address, uint8_t *data, size_t data_len);
```

This function reads from an I2C device connected to the bus.

The arguments of this function are:

- `slave_address`: Address of the slave that should send the data.
- `reg_address`: Address of the register of the slave that should send the data.
- `data`: Array to store the read data.
- `data_len`: Length of the data array.

This function returns `true` if the data was correctly read, otherwise `false`.


## API - Low level

These functions are defined when `I2C_LOW_LEVEL_API` is defined. They are not intended for direct usage, but are available, if you know what you are doing!


### `i2c_master_start_condition`

```c
bool i2c_master_start_condition(void);
```

This function sends a start condition to the I2C bus. It will be handled by all of the devices connected to the bus. It returns `true` on success, otherwise `false`.


### `i2c_master_stop_condition`

```c
bool i2c_master_stop_condition(void);
```

This function sends a stop condition to the I2C bus. It will be handled by the device currently exchanging data with the master. It returns `true` on success, otherwise `false`.


### `i2c_master_write_byte`

```c
bool i2c_master_write_byte(uint8_t data);
```

This function writes a byte to the I2C bus. It will be handled by the device currently exchanging data with the master.

The arguments of this function are:

- `data`: Byte to be written to the I2C bus.

This returns `true` on success, otherwise `false`.


### `i2c_master_read_byte`

```c
bool i2c_master_read_byte(uint8_t *data, bool last);
```

This function reads a byte from the I2C bus. It will be written by the device currently exchanging data with the master.

The arguments of this function are:

- `data`: Pointer to the memory address where the read byte should be stored.
- `last`: Boolean signaling that it is the last byte to be sent before stopping the data exchange. This **won't** send the stop condition to the bus, just send a `nack` instead of an `ack` after receiving the byte.

It returns `true` on success, otherwise `false`.


## License

This library is released under a 3-clause BSD license. See `LICENSE` file for details.
