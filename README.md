# Keyboard Driver for Windows (Kernel Mode)
This repository contains an example of a kernel-mode keyboard driver for Windows systems. The driver intercepts keyboard inputs, allowing the logging and handling of keyboard event data.

### Main Features:
1. Keyboard Input Interception: The driver attaches to the keyboard class device (`KeyboardClass0`) and captures keypress events before they reach the operating system.
2. IRP (I/O Request Packet) Processing:
    -   The driver intercepts and forwards read requests (`IRP_MJ_READ`), logging the scan codes of the pressed keys.
    -   A completion routine (`ReadComplete`) is used to process the received IRP data and determine whether a key has been pressed or released.
3. Device Detachment: Upon unloading, the driver ensures that all pending operations, such as keyboard reads, are completed before freeing the device resources.
4. Support for Multiple Operations: Through dispatch functions, the driver manages various IRP requests, ensuring that system workflow remains unaffected by its operation.

### Key Functions:
1. DriverEntry: The driver's main entry point. It sets up the dispatch functions and attaches the driver to the keyboard device.

2. DispatchPass: This function forwards IRP requests to the underlying keyboard device.

3. DispatchRead: Intercepts `IRP_MJ_READ` requests and attaches a completion routine to process keypress events.

4. ReadComplete: The routine that processes the keyboard input data upon IRP completion, printing the scan code and key state (pressed/released).

5. DriverUnload: Cleans up and detaches the keyboard device when the driver is unloaded, ensuring that all pending operations are completed.

6. MyAttachDevice: Attaches the driver to the keyboard class device (`KeyboardClass0`).

### Driver Execution:
The driver is loaded in kernel mode and attaches to the keyboard device. Each keypress is intercepted and logged through IRP handling routines.
