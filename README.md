# GoodWe
Get inverter data from a Goodwe solar inverter.

It has been reported to work on GoodWe ET, EH, BT, BH, ES, EM, DT, D-NS, XS and BP families of inverters.
It may work on other inverters as well, as long as they listen on UDP port 8899 and respond to one of supported communication protocols.

## Usage
1. Your GoodWe inverter should already be connected to your home network, and accept UDP messages at port 8899.
2. Write down your GoodWe inverter's IP address (or invoke `goodwe.search_inverters()`)
3. Install this package `pip install goodwe`
4. Connect to inverter and read all runtime data, example below

```python
import asyncio
import goodwe

async def get_runtime_data():
    ip_address = '192.168.1.14'
    port = 8899
    inverter_family = "ET" # One of ET, EH, BT, BH, ES, EM, DT, NS, XS, BP or None to detect family automatically
    comm_address = None    # Optional inverter communication address, defaults to 247 for ET/EH inverters 127 for DT/D-NS/XS inverters
    
    inverter = await goodwe.connect(ip_address, port, inverter_family, comm_address)
    runtime_data = await inverter.read_runtime_data()

    for sensor in inverter.sensors():
        if sensor.id_ in runtime_data:
            print(f"{sensor.id_}: \t\t {sensor.name} = {runtime_data[sensor.id_]} {sensor.unit}")

asyncio.run(get_runtime_data())
```
or the old way, create a processor and inverter instance:
```python
import asyncio
from goodwe import GoodWeInverter, GoodWeXSProcessor

async def get_data():
    ip_address = '192.168.200.100'
    processor = GoodWeXSProcessor()
    inverter = GoodWeInverter(inverter_address=(ip_address, 8899), processor=processor)
    data = await inverter.request_data()
    print(f'power is {data.power} at {data.date:%H:%M:%S}')

asyncio.run(get_data())
```
## References and useful links

- https://github.com/tkubec/GoodWe
- https://github.com/yasko-pv/modbus-log
- https://github.com/MiG-41/Modbus-GoodWe-DT
- https://github.com/mletenay/home-assistant-goodwe-inverter
- https://github.com/OpenEMS/openems
