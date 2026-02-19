STM32 TIMER ARCHITECTURE – ENGINEERING REFERENCE
Target: STM32F103 (Cortex-M3)

1. TIMER OVERVIEW

A timer is a hardware peripheral used to generate precise timing operations without CPU intervention.

It is commonly used for:
- Time base generation
- Periodic interrupts
- PWM generation
- Input signal measurement
- Event counting
- Motor control

Timers operate independently once configured.


2. CLOCK TREE RELATIONSHIP

System Clock Flow:

        HSI / HSE
            |
            v
           PLL
            |
            v
         SYSCLK
            |
      ----------------
      |              |
     AHB            APB
                     |
             ----------------
             |              |
           APB1           APB2
             |              |
          TIM2-4           TIM1


IMPORTANT RULE:

If APB Prescaler = 1:
    Timer Clock = APB Clock

If APB Prescaler > 1:
    Timer Clock = 2 × APB Clock

Example:
APB1 = 36 MHz
Prescaler > 1
Timer Clock = 72 MHz


3. CORE TIMER REGISTERS

PSC – Prescaler Register
Divides timer input clock.

Counter Clock = Timer Clock / (PSC + 1)

ARR – Auto Reload Register
Defines maximum counter value.
When CNT reaches ARR, update event occurs.

CNT – Counter Register
Counts from 0 to ARR.

CCR – Capture Compare Register
Used for PWM and Input Capture.


4. TIMER TIME CALCULATION

Step 1 – Counter Frequency:
F_counter = Timer Clock / (PSC + 1)

Step 2 – Period:
T_period = (ARR + 1) / F_counter

Combined Formula:
T_period = (ARR + 1)(PSC + 1) / Timer Clock


5. UPDATE EVENT

Occurs when:
- Counter reaches ARR
- Software forces update

It can trigger:
- Interrupt
- DMA request


6. TIMER TYPES IN STM32F103

Basic Timers:
TIM6, TIM7
Used for time base only.

General Purpose Timers:
TIM2, TIM3, TIM4
Support PWM and Input Capture.

Advanced Timer:
TIM1
Used for motor control and advanced PWM.


7. EXAMPLE – 1 ms INTERRUPT

Given:
Timer Clock = 72 MHz
Desired period = 1 ms

Choose:
PSC = 7200 - 1
Counter Clock = 72 MHz / 7200 = 10 kHz

ARR = 10 - 1
Period = 10 / 10000 = 1 ms


8. ENGINEERING NOTES

- Always verify APB prescaler.
- Prefer interrupt-based design.
- For servo PWM, use 1 MHz counter clock.
- Handle overflow carefully in Input Capture.
