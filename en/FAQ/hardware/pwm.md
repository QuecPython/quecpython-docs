# FAQ on PWM

### **PWM clock frequency**

The PWM clock frequency refers to the period of the PWM signal, which is the time required for a complete period of the PWM signal. It determines the frequency and updating speed of the PWM signal.

The PWM signal is composed of a series of pulses, and the width of each pulse determines the level of the signal. Moreover, the PWM clock frequency determines the repetition frequency of the pulse, that is, the period of the PWM signal. Normally, it is expressed in units of Hertz (Hz).

### **PWM resolution**

The PWM resolution refers to the accuracy or resolution capability of the PWM signal, usually expressed in bits. It determines how many different levels or values ​​the PWM signal can be divided.

The PWM signal is composed of a series of pulses, and the width of each pulse determines the level of the signal. PWM resolution indicates how many different levels the pulse width can be divided into within a complete PWM period. The higher the resolution, the more levels can be represented, allowing for better control.

The PWM resolution is usually expressed in bits, such as 8-bit, 10-bit and 12-bit. For example, 8-bit PWM resolution means that the PWM signal can be divided into 256 different levels (2^8 = 256), that is, it can be adjusted between 256 different levels of brightness, speed, or other parameters by changing the pulse width.

### **PWM Dead Zone Complementation Output**

The Dead Zone complementation is a PWM control used for current reserve switching when driving motors or other devices. When using dead zone complementation, the PWM signal usually consists of two complementary signals, one for forward current and one for reverse current. In order to avoid short circuit, there will be a dead time between the two signals. Within the dead time, both signals are low level to ensure the safety of current reverse switching.

The Dead Zone complementation can effectively reduce current switching losses and improve system efficiency and stability. In practical applications, PWM resolution and dead zone complementation will be aligned to achieve precise control and protect the safety of the circuit.
