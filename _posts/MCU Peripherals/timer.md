TIMER

**TIMER**

A timer is a hardware peripheral which counts the clock pulses to
generate periodic waveform or to capture or measure the width of given
signal or to generate time delays with higher precision and low CPU
load.

It is used to time the events or count the events, or to generate
periodic interrupts, and to generate PWM wave generation.

The timer operates with help of binary counter registers which
increments by one after each input signal. Size of this counter
determines the value of number of pulses it will accept after which
register overflows and resets to zero.

We can expand the timer i.e. increase the period of counting using
Prescaler. It is nothing but a second counter placed in front of the
main counter/oscillator and skips the number of number of defined pulses
in Prescaler. So with Prescaler for the same size of binary counter you
can count for a larger period of timer. Let's say Prescaler is set to 4,
then the main counter will skip four pulses to increment value in binary
counter register by 1.

Once the overflow value is reached in timer register then, based on user
configuration either it can generate interrupt or trigger other
peripheral or simply count the delay.

*Difference* *between* *Timer* *and* *Counter* *is* *the* *source* *of*
*counting* *pulse* *which* *increments* *the* *counting* *register.*
*If* *the* *source* *is* *time* *based* *such* *as* *oscillator* *clock*
*of* *MCU* *then* *it* *is* *timer* *and* *if* *the* *source* *is*
*some* *pulse* *(e.g.* *sensor* *or* *switch)* *based* *then* *it* *is*
*counter.*

Timer is a **finite** **state** **machine** which increments or
decrements a register with each clock cycle. Now there are different
register to operate a timer, the name of these register are obviously
MCU specific but in general we can define them as mentioned:

> 1\. INT register: This contains the initial value of timer from which
> it starts incrementing or decrementing the count value.
>
> 2\. MAX register: This contains the maximum value up to which timer
> can count or can start counting from for a 16 bit timer the max
> Register value will be 655536.
>
> 3\. RELOAD: The value up to which the counter counts or start counting
> from depending on timer mode, the maximum value of RELOAD is the MAX
> register value.
>
> 4\. Counting Register: It holds the register value of timer count and
> it starts from INT and goes up to RELOAD value or vice versa depending
> on timer mode.

Different Timer Mode: 1. Up Counting

> In this mode the timer starts counting from INT value and goes up to
> RELOAD/MAX value. 2. Down Counting
>
> In this mode the timer starts counting from RELOAD value and goes up
> to INT value and then resets back.
>
> 3\. Centre aligned (Up Down Counting/ Down Up Counting)
>
> In this mode if it is Up Down Counter then it starts from INT Value
> and increments up to RELOAD value and then decrements again till INT
> value. Down Up Counter then it starts from RELOAD Value and decrements
> up to INT value and then increments again till RELOAD value.


<img src="https://github.com/jyoti060/embedded-system/blob/main/_posts/img/img_adc_1.png"
style="width:3.15097in;height:6.00903in" />Image Credit to
[<u>TexasInstrumentTRM_MSPM0</u>](https://www.ti.com/lit/pdf/slau846)

Whenever timer switches from RELOAD to INT value (in up counting timers)
then overflow flag is set and whenever it switches from INT to RELOAD
values (in down counting timer) then underflow flag is set.

Use cases of Timers:

> 1\. Generating PWM
>
> This outputs a digital signal on MCU pins. 2. Time delays
>
> 3\. Capturing Input waveform

