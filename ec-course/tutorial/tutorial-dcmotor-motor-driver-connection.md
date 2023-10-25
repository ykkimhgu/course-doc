# Tutorial : DC motor - Motor Driver Connection

## DC Motor & Motor Driver

![image](https://user-images.githubusercontent.com/91526930/201863253-016ba03c-196b-411f-8349-201214e01865.png)





## Connection Example

The following figure is <u>an example</u> of an input/output connection method of a motor driver(LS9110s). 

* Input

  * consists of a pair of 1A and 1B.

  * one for PWM, and the other for reference voltage.

    > The reference voltage is Low or High.

- Output
  - connect the input wires(Black and Red line) of DC motor, respectively.
  - the difference between the output values rotates the motor.

![DC_motor_drive_case](https://github.com/ykkimhgu/EC-student/assets/84508106/09559200-3480-4594-ae11-cc10750def0b)

![DC_motor_drive_Config](https://github.com/ykkimhgu/EC-student/assets/84508106/623dd17b-6901-4a0b-a049-7ad102ba646d)



As you can see, there are various combinations in the way wires are connected. So, you should control the motor properly according to connecting the wires.

The example was summarized thorough actual experiments. And it could not the correct answer for you. So, apply it according to your situation.