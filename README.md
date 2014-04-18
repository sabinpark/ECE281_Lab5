ECE281_Lab5
===========

Sabin's Lab 5

## Demonstrations

### 1st Program
Demonstrated to Dr. Neebel on 18 April 2014.

### 2nd Program

## Part 1 - PRISM Setup and First Program
I followed the lab handout step-by-step and obtained my simulation shown below:

[insert simulation images with labeled controller states]

The program basically [insert what the program does]

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

## Part 2 - PRISM Assembly Programming
