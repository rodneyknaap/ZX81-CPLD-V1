# ZX81-CPLD-V1
A ZX81 project based on CPLD technology, includes equivalent logic of ZX81 ULA functions based on ZX97Lite.

Mostly in the 1990s I made some PCB designs for building your own ZX81. Since then I am also working on 286 PC based projects, preparing to create a 486 PC design without using any commercial chipset. After doing more advanced work on the 286 system, I had another look at my old projects, and plan to publish some of them in open source form.

My first idea is this project replacing all the glue logic of the ZX97 based design published by Wilf Rigter and merging all the logic into a CPLD chip. After a weekend of designing a CPLD project, soldering many wires, I have removed all the TTL ICs from my ZX81 Issue 4 project except the CMOS clock generator gates. 

After connecting the CPLD into the old PCB for my Issue 4 home etched prototype, I have debugged the project. The CPLD is connected in similar fashion as the Sinclair ULA chip, which means that the CPLD is only making use of the Dn lines and not the Dn' bus. So I had some issues where I needed to circumvent the NOP operations on the CPU and still get readable data from the memory on the Dn lines. So I created a shift register to generate timing signals. From the shift register outputs we can choose a precise timing where in the CPU cycle to get the character data from memory. And after that is clocked, the NOP is applied to the CPU so it will execute the well known NOP operations while the ZX81 video system works simultaneously.
So after debugging the timing, I was able to run the CPLD based ZX81 computer from the Dn databus only, and this frees up more pins to be able to put these to good use for other functions for expanding the ZX81 computer.

One of the future goals may be to be able to run CP/M in some form on this computer. So we will need to be able to juggle the memory decoding around in order to exchange ROM for RAM and start CP/M. I am not a programmer(yet) so I will need to look into how we can load and run CP/M. Of course, running in memory is one thing, however doing more such as CP/M DOS operations on a harddisk or floppy drive, interacting with the user etc will require some form of console. Right now it's just an idea which I am looking into.

So the basic computer is done and now I will look around what I will include additionally.
I will also look at Wilf's other ideas such as the extended character set logic, which I will test shortly.
I will need to look into how this is done in the ROM or user program, and test this out.

Kind regards,

Rodney
