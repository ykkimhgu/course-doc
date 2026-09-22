# Tutorial : DC Motor Driver Connection




# 1. Motor Driver: LS 9110S

![image](https://user-images.githubusercontent.com/91526930/201863253-016ba03c-196b-411f-8349-201214e01865.png)



## Connection

There are two combinations of connecting a DC motor to the driver. 

Choose either Case 1 or Case 2 : 


The example was summarized thorough actual experiments. And it could not the correct answer for you. So, apply it according to your situation.

The following figure is <u>an example</u> of an input/output connection method of a motor driver(LS9110s). 


**Case 1**
* MCU 

  * For Motor A:  A-1B=DIR, A-1A= PWM
  * For Motor B:  B-2A=DIR, B-1A= PWM
  
* Voltage Source
  * VCC / GND  
  > Not good idea to supply MCU 5V

* DC Motor
  * (+) Red,  (-) Black
  * The different in voltage out determines the rotation direction and speed


**Case 2**
See the following image
  


![DC_motor_drive_case](https://github.com/ykkimhgu/EC-student/assets/84508106/09559200-3480-4594-ae11-cc10750def0b)

![DC_motor_drive_Config](https://github.com/ykkimhgu/EC-student/assets/84508106/623dd17b-6901-4a0b-a049-7ad102ba646d)


## Direction and Speed
It can be confusing that increasing the PWM duty cycle increases the speed in one direction, but decreases it in the opposite direction.

Example
* DIR=0 PWM duty=0.8  --> High Speed
* DIR=1 PWM duty=0.8  --> Low  Speed 

**Solution**

```c++
float targetPWM;  // pwm for motor input 
float duty=abs(DIR-targetPWM); // DIR=1 or 0

PWM_duty(PWM_PIN, duty);
```

If the DC motor does not run under duty 0.5
* Configure motor PWM signal period at least 1kHz
* Increase VCC

---

# 2. Motor Driver: L298N Motor Driver (Channel A)

![L298N pin map](https://github.com/user-attachments/assets/6d9dac90-bb76-4b37-b93d-fc65e5923962)

## Connection

* MCU 

  * For Motor A:  A-Enable = PWM
  * For Motor B:  B-Enable = PWM
  * {IN1,IN2} = DIR options
  
    | IN1 | IN2 | DIR  |
    |-----|-----|---------|
    | 1   | 0   | Forward |
    | 0   | 1   | Reverse |

* Voltage Source
  * 5V VCC / Power GND
  * Connect Power_GND to MCU_GND (common ground)  
  > Not good idea to supply MCU 5V

* DC Motor
  * (+) Red,  (-) Black
  * The different in voltage out determines the rotation direction and speed

