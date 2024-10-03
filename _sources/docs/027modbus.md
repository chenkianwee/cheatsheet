# modbus
## pymodbus
- install pymodbus using pip install pymodbus

1. This is the example script
```{dropdown} example to read a power monitor
    import logging
    from enum import Enum
    from math import log10
    from time import sleep
    from pymodbus import pymodbus_apply_logging_config
    # --------------------------------------------------------------------------- #
    # import the various client implementations
    # --------------------------------------------------------------------------- #
    from pymodbus.client import ModbusTcpClient
    from pymodbus.exceptions import ModbusException
    from pymodbus.pdu import ExceptionResponse
    from pymodbus import FramerType

    HOST = "192.168.1.1" # change it to the modbus device of interest
    PORT = 502
    pymodbus_apply_logging_config(logging.ERROR)
    logging.basicConfig(level=logging.DEBUG,
                        format='%(asctime)s %(levelname)s %(message)s')
    _logger = logging.getLogger(__file__)

    def main() -> None:
        """Run client setup."""
        _logger.info("### Client starting")
        client: ModbusTcpClient = ModbusTcpClient(
            host=HOST,
            port=PORT,
            # Common optional parameters:
            # framer=FramerType.SOCKET,
            # timeout=5,
        )
        client.connect()
        _logger.info("### Client connected")
        sleep(1)
        _logger.info("### Client starting")
        meter_calls(client)
        write_register(client)
        meter_calls(client)
        client.close()
        _logger.info("### End of Program")

    def write_register(client: ModbusTcpClient) -> None:
        """Write a register."""
        for addr, value in ((4104, 5), 
                            (4105, 200)):
            try:
                rr = client.write_registers(address=addr, values=[value], slave=1)
                # TODO: cannot figure out why can't I use write_register instead
                _logger.info(f"write is {rr}, Value = {value}")
            except ModbusException as exc:
                _logger.error(f"Modbus exception: {exc!s}")
            if rr.isError():
                _logger.error(f"Error")
            if isinstance(rr, ExceptionResponse):
                _logger.error(f"Response exception: {rr!s}")

    def meter_calls(client: ModbusTcpClient) -> None:
        """Read registers."""
        error = False
        # check the modbus map and use that to fill in the necessary address to access the equip
        for addr, format, factor, comment, unit in ( # data_type according to ModbusClientMixin.DATATYPE.value[0]
            (16384, "f", 1,     "freq_1",    "Hz"),
            (16386, "f", 1,     "voltage",   "V"),
            (16412, "f", 1,    "power",     "W"),
            (16402, "f", 1,    "current",   "A"),
            (4104,  "H", 1,    "ct1",   "unitless"),
            (4105,  "H", 1,    "ct2",   "unitless")
        ):
            if error:
                error = False
                client.close()
                sleep(0.1)
                client.connect()
                sleep(1)
            data_type = get_data_type(format)
            count = data_type.value[1]
            var_type = data_type.name
            _logger.info(f"*** Reading {comment} ({var_type})")
            try:
                rr = client.read_holding_registers(address=addr, count=count, slave=1)
            except ModbusException as exc:
                _logger.error(f"Modbus exception: {exc!s}")
                error = True
                continue
            if rr.isError():
                _logger.error(f"Error")
                error = True
                continue
            if isinstance(rr, ExceptionResponse):
                _logger.error(f"Response exception: {rr!s}")
                error = True
                continue
            value = client.convert_from_registers(rr.registers, data_type) * factor
            if factor < 1:
                value = round(value, int(log10(factor) * -1))
            _logger.info(f"*** READ *** {comment} = {value} {unit}")

    def get_data_type(format: str) -> Enum:
        """Return the ModbusTcpClient.DATATYPE according to the format"""
        for data_type in ModbusTcpClient.DATATYPE:
            if data_type.value[0] == format:
                return data_type

    if __name__ == "__main__":
        main()
```