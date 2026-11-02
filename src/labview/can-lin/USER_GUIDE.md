# CAN/LIN Raw Bus Capture Measurement Plugin for Instrument Studio

This plugin is an easy-to-use CAN/LIN Bus capture with sorting and filtering option. It uses the Measurement Plugin SDK.

## Prerequisites

- **LabVIEW 2025 Q3**
- **Instrument Studio 2025 Q4**.
- **NI PXIe-8510** with NI-XNET-CAN and/or NI-XNET-LIN transceiver   



<div style="border: 2px solid #666666; background-color: #eeeeee; color: #000000; padding: 12px; margin: 16px 0;">
<p style="color: #000000; font-size: 1.2em; font-weight: bold; margin: 0 0 8px;">IMPORTANT</p>
<p style="margin: 0;">Plug-in performance may degrade when showing more than <strong>120 frames and/or signals on the trace window.</strong></p>  

All signals in the database are decoded and displayed in the Trace window and Signal Table.
</div>

## How to run this plugin

Once the Plug-In is installed, open up **Instrument Studio** and select **manual layout** (pay attention to the monitor resolution; a minimum 1920 x 1080 resolution should be used).

![image](<img/05.png>)

Select **CAN-LIN Raw Bus Capture** as a large Panel

![image](<img/06.png>)

The plugin UI in Instrument Studio is shown below.

The plugin can capture CAN data without a DBC file. Without one, the Trace Window displays raw data only. 
When a DBC file is loaded, messages are decoded into frames and signals only if the DBC defines the corresponding message and signals. The Trace Window displays raw data in a root/child tree with any decoded frame, signal, and value information. The Signal Table displays decoded signal information in the ID, Frame, Signal, Value, and Rate columns.

![image](<img/07.png>)

## How to configure the plugin

Select the bus type

![image](<img/08.png>)

### CAN configuration

![image](<img/09.png>)

1. Select the CAN interface
2. Select Run Mode:
    - Normal (standard operating mode; the CAN settings below apply in this mode)
    - Subordinate (the interface operates under the control of a master device)
3. Select the IO Mode:
    - CAN (Standard Controller Area Network)
    - CAN FD (Flexible Data-Rate)
    - CAN FD+BRS (Flexible Data-Rate and Bit Rate Switch)
4. Set baudrate (decimal value)
    - 125000 for 125 kbps
5. Set FD ISO Mode (optional when IO Mode is set to CAN FD or CAN FD+BRS)
    - ISO
    - Non ISO
    - Legacy ISO
6. Set FD baudrate (decimal value) (optional when IO Mode is set to CAN FD or CAN FD+BRS)
    - 1000000 for 1 Mbps
7. Turn on the termination resistor if it is needed for the bus.
8. Optionally, select a DBC file to decode CAN messages into frame and signal names and values. Only messages and signals defined in the DBC are decoded.

### LIN configuration

![image](<img/10.png>)

1. Select the LIN(Local Interconnect Network) interface
2. Turn on the termination resistor if it is needed for the bus.

*Note: The plugin connects to the LIN bus as a slave node with 128 kpbs baudrate.

## How to deploy the plugin (local only)

Build the **CAN-LIN Raw Bus Capture** and the **CAN-LIN Raw Bus Capture UI**

![image](<img/20.png>)

Copy the `src\labview\builds\CAN-LIN Raw Bus Capture\CAN-LIN Raw Bus Capture` folder under the `C:\ProgramData\National Instruments\Plug-Ins\Measurements` location.

![image](<img/21.png>)

## How to start and use the plugin

Start the plugin with the **RUN** button in the Instrument Studio.

![image](<img/11.png>)

It connects to the selected bus and starts listening. The service updates the UI every 150ms for CAN, and 100ms for LIN. The connection LED turns on.
The Trace Window displays raw bus data and, when a DBC file is loaded, any matching decoded frame, signal, and value information in a tree. CAN capture works without a DBC; without one, no decoded frame or signal information is shown. The Signal Table displays the ID, frame, signal, value, and rate without the raw data. The Status area displays the current bus status. The DBC Information section shows whether the DBC file is valid and the number of frames and signals it contains.

![image](<img/12.png>)

The user can interact with the plugin by the control area such as sorting, filtering, pause / resume and clear the trace table or save log files in TDMS or CSV format.

![image](<img/13.png>)

### Sorting
The default order of the messages is by timestamp, it can change by the **Sort by ID** checkbox.

![image](<img/14.png>)

### Filter
Use the headers of the trace table to set conditions what to filter.

In the Trace Window, if a root row matches the filter, all of its children are shown. If a child field (Frame, Signal, or Value) matches, its root row and only the matching child rows are shown. 
In the Signal Table, only signal rows that match the filter are displayed.

The Value and Rate columns may include units, such as V, km/h, or Hz. When filtering these columns, enter only the value and omit the unit (for example, `Rate < 99`).

The filter expression uses the following syntax:

- Sets of three arguments separated by logical operators.
- Spaces are used to separate keywords.
- Each set of three arguments follows the pattern: **Field, Operator, Value**.
- Quotations must be used when filtering for multiple words, or strings with a space.
- The hexadecimal prefix (0x) is needed when filtering for hexadecimal values.


**Example:**

    Value < 11 OR ID == 0x720

    Type == "CAN Data"

    ID == 0xAA

![image](<img/15.png>)

![image](<img/16.png>)

![image](<img/17.png>)

**Invalid inputs include:**
- Standalone logical operators (e.g., `AND`, `OR`)
- Terms by themselves
- Argument sets not equal to three in length
- Parentheses

If an invalid filter is entered, a warning will be generated.

### Save trace to log file (TDMS, CSV)

The user can set a path in the control area and save the trace data to CSV or TDMS file with the **SAVE** button. The file name auto generated as {BusType_tracelog_YYYYMMDD_HHIISSTUUU}
The saved file contains the most recent 10,000 raw data rows retained in the background.
User got response message at the bottom of the Status area.

### Pause / Resume capture

Use this button to pause the trace table updates. Messages received while paused are not queued for later display and are lost. After you resume, only newly received messages appear in the tree.

### Auto Scrolling

Auto-scrolling is available in the Signal Table, but not in the Trace Window tree. In the tree, new data appears at the bottom. If a row with the same ID already exists, it is updated instead of adding a new row.

When the Signal Table is not paused, it automatically scrolls to the latest packet. You can disable auto-scroll by moving the scrollbar up. To re-enable it, scroll to the bottom of the Signal Table. With continuous incoming traffic, reaching the bottom can be difficult. You can pause before scrolling, but messages received while paused are lost.

When you have more than 10k packets in the table, this method does not matter. We suggest pausing if you want to spend time searching for specific packets for ease of use.

## Stopping the Plug-In

When you stop the plug-in, any packets that have not yet been processed (for example, packets that are not visible because the plug-in is currently paused) will be permanently lost.
For this reason, only stop the plug-in when you are certain that there is no remaining data you still need to monitor.
