# TP36onESP32
How to read temperature from TP36 using ESP32.

For several days I had been trying to read the temperature of a TMP36 from my ESP32 without success.
The thing is that on Arduino the sensor narrated perfectly.

Still doubtful, I started looking and found my official Arduino guide. There the solution was easy:

'''
int sensorVal = analogRead(A0);
float voltage = sensorVal / 1024 * 5;
float temp = (voltage - 0.5) * 100;
'''

But on ESP32 it doesn't work, why?

The analog pins of the Arduino read a value from 0 to 1023 (1024 values) based on the voltage on the pin (0v - 5v).
The analog pins of the ESP32 read from 0 to 4095 (4096 values) based on the voltage on the pin (0v - 3.3v).

So how do you go about it?

My idea was pretty easy: I use ESP values instead of Arduino ones.

Let's take an example:
'''
int sensorVal = analogRead(A0); // 567
float voltage = 567 / 4096 * 3.3; // 0.4568 V
float temp = (0.4568 - 0.5) * 100; // -4.31 °C
'''

Something went wrong, it's not that cold.

So I said, let's try to make a proportion!
'''
sensorValARD : 1023 = sensorValESP : 4095
sensorValARD = sensorValESP * 1023 / 4095

sensorValARD = 567 * 1023 / 4095; // 142
'''

Now we have a fake reading, as if it were Arduino: let's use its formulas now.
'''
float voltage = 142 / 1024 * 5; // 0,69 V
float temp = (0,69 - 0.5) * 100; // 19 °C
'''

Bingo!

- Why does the ESP formula not work, which one would be correct, where is the error in the reasoning?
