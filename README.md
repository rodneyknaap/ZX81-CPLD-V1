# ZX81-CPLD-V1
A ZX81 project based on CPLD technology, includes equivalent logic of ZX81 ULA functions based on ZX97Lite.

![A photo of the hand soldered CPLD prototype using the ZX81 Issue 4 PCB](Img_5525s.jpg)

## Purpose and permitted use, cautions for a potential builder of this design
This project was created for historical purposes out of love for historical computing designs and for the purpose of enabling computing enthousiasts with a sufficient level of building and troubleshooting expertise to be able to experience the technology by building and troubleshooting the hardware described in this project. Due to the level of this project, it may be suitable as a project for students to get into. If there are any questions from teachers who like to teach about this technology I would be happy to answer them. It may be really interesting to analyse the elaborate and complex CPU timing and 8 bit to 16 bit data byte translation and DMA mechanisms in an educational setting.

Besides the GPL3 license there are a few warnings and usage restrictions applicable:
No guarantees of function or fitness for any particular or useful purpose is given, building and using this design is at the sole responsibility of the builder.

Do not attempt this project unless you have the necessary electronics assembly expertise and experience, and know how to observe all electronics safety guidelines which are applicable.

It is not permitted to use the computer built from this design without the assumption of the possibility of loss of data or malfunction of the connected device. To be used strictly for personal hobby and experimental purposes only. No applications are permitted where failure of the device could result in damage or injury of any kind.

If you plan to use this design or any part of it in new designs, the acknowledgement of the designer and the design sources and inspirations, historical and modern, of all subparts contained within this design should be included and respected in your publication, to accredit the hard work, time and effort dedicated by the people before you who contributed to make your project possible.

No guarantee for any proper operation or suitability for any possible use or purpose is given, using the resulting hardware from this design is purely educational and experimental and not intended for serious applications. Loss of data is likely and to be expected when connecting any storage device or storage media to the resulting system from this design, or when configuring or operating any storage device or media with the system of this design.

When connecting this system to a computer network which contains stored information on it, it is at the sole responsibility and risk of the person making the connection, no guarantee is given against data loss or data corruption, malfunctions or failure of the whole computer network and/or any information contained inside it on other devices and media which are connected to the same network.

When building this project, the builder assumes personal responsibility for troubleshooting it and using the necessary care and expertise to make it function properly as defined by the design. You can email me with questions, but I will reply only if I have time and if I find the question to be valid. Which will probably also lead to an update here. I want to primarily dedicate my time to new project development, I am not able to do any user support, so that's why I provide the elaborate info here which will be expanded if needed.

# Acknowledgements

This project was inspired by:
- Sinclair computers in the 1980s
- Wilf Rigter who helped me with my PCB designs in the 1990s
- the German ZX-Team who inspired me a lot by sending me their Magazin and kind letters and messages. Particularly Peter Liebert Adelt and Kai Fischer with whom I have had very pleasant and memorable contact in the 1990s years.

# Concept of the project
Mostly in the 1990s I made some PCB designs for building your own ZX81. Since then I am also working on 286 PC based projects, preparing to create a 486 PC design without using any commercial chipset. After doing more advanced work on the 286 system, I had another look at my old projects, and plan to publish some of them in open source form.

My first idea is this project replacing all the glue logic of the ZX97 based design published by Wilf Rigter and merging all the logic into a CPLD chip. After a weekend of designing a CPLD project, soldering many wires, I have removed all the TTL ICs from my ZX81 Issue 4 project except the CMOS clock generator gates. 

After connecting the CPLD into the old PCB of my Issue 4 home etched prototype where I removed all the TTL ICs except the clock generator, I have now fully debugged the project. 

The CPLD is connected in a similar fashion as the Sinclair ULA chip, which means that the CPLD is only making use of the Dn lines and not the Dn' bus of the ZX81 based system. So this fact presented some issues where I needed to circumvent the NOP operations on the CPU which essentially were blocking access to character data on the Dn lines. I needed to get character data from the memory through the Dn lines and clock it into the character register. So I created a shift register to generate timing signals from the faster pixel clock. From the shift register outputs we can choose a precise instance in the CPU cycle to load the character data from memory. The ZX81/ZX97 memory is controlled in such a way that when the chip is selected, it also automatically outputs the data on the Dn' bus. And after that data is clocked into the character register first, the NOP signal is then immediately applied to the CPU so it will execute the well known NOP operations while the ZX81 video system works simultaneously. This modification turned out to be everything needed to operate the ULA functions on the CPU side of the databus. There was still a small problem with D6 and D7 from memory, but I got those bits by also clocking them at the same time with the character bits. The clocked data is then also fed back into the logic used to decode the NOP operation, on time in the same CPU cycle. 

So after debugging the timing, I was able to run the CPLD based ZX81 computer from the Dn databus only, and this frees up 8 more pins to be able to put these to good use for other functions for expanding the ZX81 computer. What I did was first to have the CPLD on both the Dn and Dn' bus, and then moving sections onto the Dn bus only, and solving the problems which were then occurring as described above. I did some tweaking of the serial video stream sync logic to line up the vertical lines a bit more straight, but for now I will wait with these kind of modifications when I have a decent PCB made so we will have the real impedance of all the traces and logic, and then I will look at what needs some tweaking in the CPLD, if any.

One of the future goals of this recent ZX81 work may become to be able to run CP/M in some form on this computer. So we will need to be able to juggle the memory decoding around in order to exchange ROM for RAM and start CP/M. I am not a programmer(yet) so I will need to look into how we can load and run CP/M. Of course, running in memory is one thing, however doing more such as CP/M DOS operations on a harddisk or floppy drive, interacting with the user etc will require some form of console. Right now it's just an idea which I am looking into. CP/M will be better if we have more RAM available so I may look into some bank switching logic. This may require some register TTL IC since the CPLD is not great in terms of how many register bits you can store inside it.

Right now I have some ATF1508AS CPLDs so I will use these to base this project on. Probably I will directly solder the chip on the PCB for stability reasons. Sockets are really not so great for any computer because they are likely to introduce contact based malfunctions especially if we only can obtain low quality stuff these days.

So the basic computer is done and now I will look around what I will include additionally. Only making a ZX81 type computer is a bit limited so I will look at other things to integrate.
I will also look at Wilf's other ideas such as the extended character set logic, which I will test shortly.
I will need to look into how this is done in the ROM or user program, and test this out.
And one of my follow up ideas will be to design a complete ZX97 computer according to Wilfs original idea in the 1990s years.

Regarding this project, I will start a draft PCB design and put everything on the board, just to be able to judge how much space will be available for integrating some useful expansions and features, or possibly try to make CP/M possible. The most useful form of CP/M would be realized if we could integrate ZX81 video generation in some form into CP/M. If someone is able to assist in this regard, please contact me.

---------------------------------------------
Update 26-4-2025 to REV0 quartus project zip:
- updated the polarity of character video stream, set default to ZX81 screen: black characters on white background
- connected ZX81 keyboard
- tested more extensively, wrote a few demo programs in BASIC
- tested on a CRT TV converted to monitor to check SYNC, LOAD and SAVE display if this is showing correctly, which it is
REV0 is now reasonably well tested
![The_PCB_rev_0_design_in_progress](ZX81_Issue5_Rev0_Progress.png)
---------------------------------------------
Update 2-5-2025 to quartus package "002_ZX81_CPLD_CHR$128_WORKING@.zip":
- CHR$128 is fully working
- more elaborate I/O decoding (may be removed if this breaks some games)
- reworked the timing inside the CPLD according to Wilf Rigters ZX97 explanation
- done more work on analog tape input circuits to support safe line level amplitude loading of programs using the CPLD
![ZX81/ZX97 video timing diagram based on Wilfs explanation](ZX81_ZX97_video_timing.png)
According to Wilf explanation we have these timing instances:

1. Each character code (CHR$) byte in DFILE is addressed by the CPU PC, on
the
   rising edge T2 data is loaded from DFILE into the 74HC574 : bits 0-5
   and 7 into 7 bits.
2. On the falling edge of T2, the NOP circuit forces all CPU data lines to
zero.
3. On the rising edge of T3 the low data lines are interpreted by the CPU
   as a NOP instruction.
4. During T3/4, the CPU executes the Refresh cycle and ROM address lines are
   generated with I register on A9-A15, the CHR$ latch on A3-A8, and the
   ROW counter on address lines A0-A2.
5. On the rising edge of T1, pattern data from the EPROM is loaded into
   video shift register and 8 video pixels are shifted out at 6.5MHz
6. If bit 7 of the CHR$ latch equals 1, then the serial video data is
inverted.
7. The CPU increments the program counter and fetches the next character
code.
8. This repeats until a HALT is fetched.
9. HALT opcode bit 6 = 1 and is therefore executed (no NOP)
10.The SYNC timebase generates a HSYNC pulse independend of the CPU timing
and
   the ROW counter is incremented
11.The halted CPU continues to execute NOPs, incrementing register R and
   samples the INT input on the rising edge of each T4.
12.When A6, which is hardwired to INT, goes low during refresh time,
   (bit 6 of the R reg = 0), the Z80 executes the INT routine (below 32K)
13.CPU returns from INT and resumes "excution" of DFILE CHR$ codes.
14.The process repeats 192 times and then INT routine returns to the main
   video routine, turns on the NMI latch and switches back to the
   application code.
---------------------------------------------
![The current CPLD schematic including CHR$128 UDG support](ZX81_CPLD_schematic_including_CHR$128_support.gif)
---------------------------------------------

Next update(in development):
- use an external transceiver for the keyboard scanning outputs instead of diodes
- taking first steps for supporting CP/M and being able to fully disable all ZX81 related circuits during CP/M runtime (for the moment)
- performing tests with ZX81 mode enabled and disabled, testing if ZX81 can return in stable manner

Kind regards,

Rodney
