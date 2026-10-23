# FAQ on Software Development

### **Does thread on QuecPython have priority?**

The threads has no priority before. If callbacks and main functions are involved, the priorities are are shown as below: callback > thread > main function.

### **What is the minimum unit of delay for each platform?**


Microseconds. utime.sleep_us()

### **Can accurate us delay be achieved?**


If the `utime.sleep` delay in the code logic is used to count, the time deviation will occur probably. This delay time is not precise since the accurate delay requires a timer, and the millisecond-level delay and microsecond-level delay interfaces will be more inaccurate, which can't be used in timing too. 

### **Implementation on callback function of QuecPython**

1. Register callback function: You need to use relevant functions or methods to register the callback function to the specified event.
2. Event triggering: When a specific event occurs, the system will detect and trigger corresponding event.
3. Call the callback function: Once the event is triggered, the system will automatically call the callback function and transfer the relevant parameters to the callback function.
4. Execute callback function: In the callback function, you can perform event-related operations and logic.

### **What should we pay attention to when implementing the callback function?**

1.  Similar with an interrupt with the highest priority, the callback cannot process unduly complex business, and the processing time should not be longer than the thread (sleep), otherwise other threads may be blocked and unable to execute.
2. The uploaded parameters shall be matched.
3. Make the code readable and maintainable, write it clearly, and add appropriate comments to the callback function and related code to make it easy to understand and modify.


