ECE281_Lab5
===========

Sabin's Lab 5

## Demonstrations

### 1st Program
Demonstrated the 1st program demo on the FPGA to Dr. Neebel on 18 April 2014 in class.

### 2nd Program
Demonstrated the 2nd program demo on the FPGA to Dr. Neebel on 25 April 2015 at 0845.

### 3rd Program (The super duper cool program...)


## Part 1 - PRISM Setup and First Program
I followed the lab handout step-by-step and obtained my simulation shown below:

#### 0 - 100 ns (LDAI, ADDI, OUT)
![alt text](https://raw.githubusercontent.com/sabinpark/ECE281_Lab5/master/sim_1.PNG "sim 1")

#### 100 - 200 ns (JN, ADDI, OUT)
![alt text](https://raw.githubusercontent.com/sabinpark/ECE281_Lab5/master/sim_2.PNG "sim 2")

#### 900 - 1000 ns (ADDI, OUT, JUMP)
![alt text](https://raw.githubusercontent.com/sabinpark/ECE281_Lab5/master/sim_3.PNG "sim 3")

## What the Program Does
The program basically loads in a value of 8 to the accumulator then adds 1.  The value if output into output port 3.  As long as the current accumulator value is negative (from 8 to F), then the program will jump back to adding 1 and output the new value to output port 3.  If the value hits 0, then the program will display 0 and then go into an infinite loop.

## Program Setup and Test

I then copied the necessary files from Lab 3 into Lab 5.  In the *Nexys2_top_shell.vhd* file, I declared and instantiated my PRISM module.

Following the directions, I set my instantiation as follows:
```vhdl
	Inst_PRISM : PRISM PORT MAP(
		Clock => Clockbus_Sig(22),
		Reset_L => reset_low,
		--Control_Bus => ,
		Input_0 => switch(3 downto 0),
		Input_1 => switch(7 downto 4),
		Input_2 => "0000",
		Input_3 => "0000",
		Output_0 => nibble3,
		Output_1 => nibble2,
		Output_2 => nibble1,
		Output_3 => nibble0
	);
```

After several attemps at messing around with the clock cycle, I eventually got the program to run with the Clock set to *Clockbus_Sig(22)*.  Anything below that did not work, and everything above was too slow for my liking.  I demonstrated my program and proceeded to work on Part 2.

On the FGPA, the program did display the 9, then incremented by 1 until the final output displayed a 0.  The program did remain at zero as expected. 

## Part 2 - PRISM Assembly Programming

For Part 2, I was assigned to program a counter that starts at 00 and goes all the way to 99, then loops back to 00.  If the input was negative (from 8 to F), then the counter would decrement instead.  Also, if the current count was at 00, the program would then decrement back to 99.  

The input to check was from input port 0.  The outputs were output ports 1 and 2.  

Instead of creating a flow chart, I dove straight into the program and began coding.  At first it was difficult to manage the different ideas going off in my head all at once, but then I became used to the syntax and coding methodology, and was able to turn all the madness into a funcitonal program.  

### Methodology
First, I loaded 0 into the acc.  I then stored 0 into the ones digit and tens digit memory values.  I proceeded to output those values.  Next, I check what the input value is.  If it is positive, I go to the increment loop; otherwise, I go to the decrement loop.  From there, I load the ones digit, increment it, and store it.  I also add a 7 to the ones digit to check if the current ones digit is a 9 (7+9 gets me to 0 in hexadecimal).  From there, I use a Jump Zero to increment the tens digit by 1 and reset the ones digit back to 0.  Otherise, I continue incrementing the ones digit.  When I check if the ones digit is a 9, I also check if the tens digit is a 9.  This means that the counter would be at 99.  And so, then the program will go to a different loop that will reset the ones and tens digits back to 0 (gives the effect of resetting the counter from 99 to 00).  

Decrementing was even easier.  I still checked the input value at the beginning of each loop.  However, instead of adding 7 to check if the current ones digit was a 9, I simply had to use a Jump Zero to know that my ones digit reached a X0.  From there, I decremented the tens digit by ones and reset the ones digit back to 9.  Another loop I needed was a 00 to 99 loop, which obviously reset the ones and tens digits back to 99.

### Testing Program 2
I re-compiled, synthezied, implemented, and downloaded my program.  I ran the program on the FPGA.  As expected, the SSEG incremented from 00 to 01 to 02 and so forth to 99. At 99, the program went back to 00.  I then flipped the switch for input 0 so that the input was 8.  The program then proceeded to decrement by 1 from 00 to 99 and all the way down.  The program worked!

## A/B Functionality
For the A/B functionality, I decided to make a multiplication calculator.  My idea was to take two numbers ranging from 0 to 15 and multiply then together.  For simplicity, I decided to make the max correct output to be limited to two digits and within the max display value of F9, or 159.  For example, I could multiply 9\*9 tp get 81.  I could also multiply 12\*13 to get 156 (with the output showing F6).  Anything beyond 159, I decided not to account for.

### Methodology

The overall idea of this program is to set an initial answer to 0, then add the value of A, B number of times.  The addition of A will happen in increments of 1.  Those increments of 1 will be kept tracked by *A counter*.

First, I took in the inputs and stored them as A and B.  I also stored a value called A Counter (set initially equal to A).  From there, I loaded the value of B and checked if B was 0.  If so, the program would go to the very end and display the results.  Otherwise, the program would load the value of A Counter and check if A counter is 0.  If so, the program will reset A counter and attempt to add A again.  If not, then the program will continue to add A in increments of 1.  Similar to the previous program, if A is 9, then the program will increment the tens digit by one and reset the ones digit back to 0.  At the very end, when B reaches 0, the program will know that A has indeed been added B number of times.  The program will loop to the very beginning and take in possible new input values to multiply together.

### Testing
I ran the program on the FPGA and started with 0 times all combinations of B.  The display was zero as expected.  I did the same thing with A = 1 and the display corresponded to the value of B.  I continued the testing with the other combinations and everything worked as expected.

## Trouble Shooting
### Programs 2 and 3
I had a very frustrating time with the PRISM simulator in that everything ran very,very slowly.  I had to wait about 6 seconds before a number would count up from 00 to 01.  This made it very time-consuming to test my program.  Fortunately, after I was all done with my 2nd program, I found a solution to this madness (of course it had to be after I finished...).  What I normally did was run my program using the .jar file.  I then tried the .exe file.  Both of them were very slow.  By chance, I opened the .bat file, then ran my program.  It was exponentially faster.
