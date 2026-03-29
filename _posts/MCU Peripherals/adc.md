
ADC: ANALOG TO DIGITAL CONBVERTER

First of all let's start with what is meant by digital and analog signal. Analog signal are those which represent data in analogous or continuous fashion, almost all the real life signal are analog in nature. On the other hand, digital signals are those which represent data in discrete manner. It is easier to process digital signal compared to analog because of the following aspects:


|Feature|Analog signal|Digital Signal|
| - | - | - |
|Complexity|Limited by hardware design|Complex algorithm are implemented in software|
|Flexibility|Task Specific|High Adaptability because of programmability|
|Signal Processing|Specialized|DSP|
|Noise Immunity|Highly Susceptible|Tolerant|
|Security|Difficult to encrypt|Easily encrypted|
|Modulation|Amplitude Modulation, Frequency Modulation|Phase shift Keying (PSK), Quadrature Amplitude Modulation (QAM).|
|Miniature|Limited|High Density, (Moore's Law)|

So it is easy to process and handle digital signal than analog signal,  but the problem is that most of the real life signals are analog in nature and the device which process these signals operates on digital signal. Here the ADC and DAC comes in picture.

<p align="center">
<img src="https://github.com/jyoti060/embedded-system/blob/main/_posts/img/img_adc_1.png" alt="Signal Processing" width="700" height = "500"/>
</p>

ADC  stands for Analog to Digital Converter, it is a hardware module which takes an analog signal as input and converts it into digital values. The block diagram of ADC is as follows 

<p align="center">
<img src="https://github.com/jyoti060/embedded-system/blob/main/_posts/img/img_adc_2.png" alt="Block Diagram of ADC" width="900" height="500"/>
</p>

The first block of an ADC is sample and Hold circuit. The sample and hold circuit is an electronic circuit consisting of a MOSFET switch, Capacitor and operational amplifier, and is used to sample and the input signal and hold or freeze it for some time which is mainly helpful in data procurement. To understand it briefly, the MOSFET samples the input signal entering from drain terminal and in ON state charges the capacitor, which holds the voltage state when switch turns off, the Op-Amp mainly functions as gain amplifier and capacitor faces a high impedance because of that and do not discharge and hold up the charge for some time and this time is called holding time. 

<p align="center">
<img src="https://github.com/jyoti060/embedded-system/blob/main/_posts/img/img_adc_3.png" alt="Circuit Diagram for Sample Hold Ckt" width="600" height="500"/>
   
   [credit: GFG](https://media.geeksforgeeks.org/wp-content/uploads/20240206124751/ww.webp) 
</p>


The on off state of MOSFET is controlled through a control signal whose frequency should be more than twice that of the sampling signal, more the frequency better the accuracy of the discrete signal as per Shannon - Nyquist Theorem which can be represented as fsample <= 2fcontrol. Even choosing too high control signal frequency might lead to picking up of noise and adding more high precision filter and CPU overload. So the right selection of control signal frequency depends on dynamics of system and its sensing requirements. 

To determine the performance of sample and hold circuit different parameter are evaluated:

1. Tap, Aperture Time: This is the time required by capacitor to switch from sampling to holding mode. 
1. Taq, Acquisition Time: The time required by capacitor to charge up to the input voltage.
1. Voltage Droop: the drop in voltage in holding mode because of charge spillage through capacitor.



<p align="center">
<img src="https://github.com/jyoti060/embedded-system/blob/main/_posts/img/img_adc_4.png" alt="Simplified Sample Hold Circuit" width="400" height="300"/>
</p>

The next part is Quantization; Before deep diving in quantization, you should have a understanding of ADC resolution and what does n-bit ADC specifies. The resolution describes number of discrete analog values for the given voltage range. For a n-bit ADC resolution will be 2^n.

Let's say the operating voltage range of ADC is 0-10V, Now let's see from table the values we will have for different ADC.

|n-bit AD|<p>CTotal discrete level</p><p>(Resolution)</p>|Discrete Voltage Values|Step Size|
| - | - | - | - |
|1|2|0, 10|10|
|2|4|0,3.33,6.66,10|3\.33|
|3|8|0,1.42,2.85,4.28,5.71,7.14,8.57, 10|1\.42|

Quantization convert the discrete time interval or time scale sampled signal to a digital signal. The quantization scheme determines the type of ADC.  It is where rounding off or truncation of analog signal lying between two Q-level takes place. Quantization simply means rounding off the analog input to nearest digital step and this rounding off generates an unavoidable error called quantization error. 

Quantization step Size is defined as Δ = Vfs/((2^n)-1); where Vfs is full scale voltage range, in the above example Vfs = 10V.

Quantization error = 1/2(LSB) or 1/2(step\_size)

Higher the bits , higher the resolution, lower the quantization error, but increasing bits also increases the memory size , so it is trade-off between accuracy and memory.

<p align="center">
<img src="https://github.com/jyoti060/embedded-system/blob/main/_posts/img/img_adc_5.png" alt="Quantization" width="600" height="400"/>
</p>
Apart from quantization and resolution there are different factor which determines the accuracy of ADC:

1. Quantization Noise Power: RMS value of quantization error, which is directly proportional to (Δ ^ 2)/12. So, 𝑣n= Δ/ √12

2. SNR: Signal to Noise Ratio, for an ideal ADC with full scale sine wave input is, SNR = (6.02\*n +1.76) dB. It shows that each additional bit of resolution improves the signal by 6dB.
3. EONB - effective Number of Bits - indicates the actual usable resolution of an ADC based on its SNR. ENOB = (SNR - 1.76)/6.02. In ideal case EONB will be same as number of bits but due to noise and non-idealness , the value in real time is lesser than what is said e.g. for a 12-bit ADC with SNR 70, EONB will be 11.3 which is close to 11 bits.

4. Oversampling - One way to reduce noise is over sampling, i.e. sampling the signal at much higher rate than nyquist frequency, and then digitally filtering the circuit we can push noise out of band of interest. Oversampling by a factor of 4 improves SNR by 6dB, which is equivalent to adding extra bit to ADC, so we can achieve higher resolution at low bandwidth.
