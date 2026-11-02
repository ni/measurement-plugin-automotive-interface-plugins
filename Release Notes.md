# Release Notes

## New Features

Updated the CAN/LIN Raw Bus Capture plugin with the following features:
* Added support for DBC files.
* Added a DBC Information section that shows whether the loaded DBC file is valid and displays its frame and signal counts.
* Added Subordinate run mode.
* Added separate Trace Window and Signal Table views. The Trace Window displays raw data and DBC-decoded frame, signal, and value information in a root/child hierarchy. Each root row has a unique ID; incoming data for an existing ID updates that row with the latest data instead of creating a duplicate. The Signal Table displays the decoded signal data in a human-readable format (ID, Frame, Signal, Value, and Rate).
* CSV and TDMS exports include up to the 10,000 most recent raw data rows.

Please see documentation [link](https://github.com/NI-Measurement-Plug-Ins/hardware-validation/tree/main?tab=readme-ov-file#plugin-documentation) to access the index to each plugin README or USER GUIDE.

## Dependencies 
* InstrumentStudio 2025 Q3 or later
* LabVIEW 2025 Q4 or later
* Measurement Plugin SDK 3.5.0.2
* See individual measurement README's for NI driver version support.


## Limitations


## Known Issues:
These issues are organized by plugin:

### Auto Ethernet plugin
 - The string filter activates when the user clicks away from the text field. There is no indicator that the string filter is processing, and it may take a second to apply.
 - The standard Ethernet mode is not yet implemented.
 - Re‑enabling auto‑scroll while the table is continuously updating can be tricky. If you are having issues reaching the bottom, try pausing the plugin, scrolling the table to the bottom, and then unpausing to re‑enable auto‑scroll.

### CAN-LIN Raw Bus Capture plugin
 - All signals in the database are decoded and displayed in the Trace window and Signal Table.
 - Plug-in performance may degrade when showing more than **120 frames and/or signals** on the trace window.
 - The string filter activates when the user clicks away from the text field. There is no indicator that the string filter is processing, and it may take a second to apply.
 - Re‑enabling auto‑scroll while the table is continuously updating can be tricky. If you are having issues reaching the bottom, try pausing the plugin, scrolling the table to the bottom, and then unpausing to re‑enable auto‑scroll.
