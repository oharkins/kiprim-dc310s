# Serial Commands for KIPRIM DC310S Voltage/Current Supply

I recently purchased a KIPRIM Power Supply DC310S and reverse engineered the following commands for USB serial control.

Feel free to contribute additional commands via pull request.

**EDIT**: It appears to be a rebranded version of [OWON SPE3103](https://www.owon.com.hk/products_owon_spe_series_%7C_spe-u_series_1_ch_dc_power_supply). [A documentation of serial commands](http://files.owon.com.cn/software/Application/SP&P_Series_Single_Channel_DC_Power_Supply_Programming_Manual.pdf) is available.

## Web Controller Interface

This repository includes `index.html`, a browser-based controller for the DC310S power supply using the Web Serial API.

### Features

- Real-time voltage, current, and power monitoring
- Live graphing with interactive tooltips
- Output control (on/off)
- Set voltage and current with fine adjustment controls
- Protection limits (OVP/OCP) configuration
- Energy consumption tracking (Wh/mWh)
- Data persistence using IndexedDB
- Manual command interface for testing
- Adjustable polling interval
- Dark themed industrial UI

### Usage

1. Open `index.html` in a Web Serial API compatible browser (Chrome, Edge, Opera)
2. Click "CONNECT" and select your DC310S serial port
3. The interface will automatically query and display current settings
4. Use the controls to adjust voltage, current, and limits
5. Toggle output on/off as needed
6. View real-time data on the live graph

### Browser Compatibility

Requires a browser with Web Serial API support:
- Chrome 89+
- Edge 89+
- Opera 75+

Note: Firefox and Safari do not currently support the Web Serial API.

## Serial Connection Settings

|Setting|Value|
|---|---|
|baud rate|115200|
|data bits|8|
|parity bit|none|
|stop bit|1|

## Read/Get Commands

Get commands are tailed with a line feed (0x0a). While the [official software](https://bit.ly/37Bwt92) appears to apply upper-case for the first three letters, commands appear to be case insensitive.

Returns are ASCII tailed with CR LF (0x0d 0x0a).

Invalid commands appear to return `ERR`.

|Command|Return|Description|
|---|---|---|
|`*idn?`|`KIPRIM,<model, eg.DC310S>,<serial no.>,FV:Vx.x.x`|get manufacturer, model, serial no, firmware version|
|`output?`|`ON` or `OFF`|get if power supply is active|
|`measure:current?`|(x)x.xxx|get measured current|
|`measure:voltage?`|(x)x.xxx|get measured voltage|
|`current?`|(x)x.xxx|get set current|
|`voltage?`|(x)x.xxx|get set voltage|
|`current:limit?`<br />`curr:lim?`|(x)x.xxx|get measured current|
|`voltage:limit?`<br />`volt:lim?`|(x)x.xxx|get measured voltage|

## Write/Set Commands

Get commands are tailed with a line feed (0x0a). While the [official software](https://bit.ly/37Bwt92) appears to apply upper-case for the first three letters, commands appear to be case insensitive.

ToDo: Not sure if there is any return for valid/invalid set commands. At the moment, I don't see any.

|Command|Description|
|---|---|
|`output <0/1>`|set power supply off/on|
|`current (x)x.xxx`|set current|
|`voltage (x)x.xxx`|set voltage|
|`current:limit (x)x.xxx`|set current limit|
|`voltage:limit (x)x.xxx`|set voltage limit|

## Untested commands from OWON pdf (TODO)

* `measure:power?`
* `*RST`
