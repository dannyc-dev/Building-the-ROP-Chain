# Smashing the Stack Part 2
# Building the ROP Chain
### By: Danny Colmenares 
#### Twitter: @malware_sec

## Welcome back! 
This is the second part to our Smashing the Stack series. In [Part 1](https://malwaresec.github.io/Stack-Based-Buffer-Overflow/) we focused on controlling execution flow and overwriting our return address using **free and open source tools**: 

![alt text](screenshot/15.png)

It's important that we fully understand how we developed the exploit in Part 1 because we will be building off it in this writeup and adding in our ROP chain to invoke our shell and get our flag (remember we're using the SECCON CTF [baby_stack](https://github.com/MalwareSec/Stack-Based-Buffer-Overflow/blob/master/baby_stack-7b078c99bb96de6e5efc2b3da485a9ae8a66fd702b7139baf072ec32175076d8.dms) challenge)

This is how our exploit looks so far: 

![alt text](screenshot/14.png)

Just to recap, our first three lines are the offset we use for the padding to our printf functions where we pass an address and how many bytes to read after that address (we picked an arbitray 0x8 so it reads 8 bytes starting at 0xc82003bd60). We found those offsets using pattern_create and pattern_offset functions (or whichever techniques you feel most comfortable with). 

Our current payload looks like this:

    paylod = 'A' * off_printf1 + p64(0xc82003bd60) + p64(0x8) + 'A' * off_printf2 + p64(0xc82003bd60) + p64(0x8) + 'A' * 
    off_retaddress + p64(0xdeadbeef)

In our final payload we will change p64(0xdeadbeef) with our ROP chain to bypass the DEP/NX protection on our executable: 

![alt text](screenshot/3.png)

Let's talk a bit about why we need this method. So like we briefly discussed in Part 1, we know we have a GO executable with DEP/NX enabled so a traditional ret2libc method and traditional buffer overflows where you set RIP to RSP+offset and immediately run your shellcode won't work. To get around this we use a clever method called Return Oriented Programming (ROP).

This writeup is not meant to be a tutorial on Return Oriented Programming (there are much better resources online than anything I could write) but I do want to make sure that we're on the same page about some key concepts and terminology so that our example further down makes sense. So the main idea behind Return Oriented Programming is the utilization of small instruction sequences available in either the binary or libraries linked to the application called gadgets. 

    These gadgets are chained together via the stack, which contains your exploit payload. Each entry in the stack corresponds
    to the address of the next ROP gadget. Each gadget is in the form of instr1; instr2; instr3; ... instrN; ret, 
    so that the ret will jump to the next address on the stack after executing the instructions, thus chaining the gadgets
    together.
    
There are multiple things that we can do with gadgets. Basically we can execute any instruction if the right instruction sequence is found. For example, if we want to load from memory we could use: 

    MOV RCX, [RAX]; ret
    
This will move the value located in the address stored in RAX, to RCX. We will be using a gadget like this in our example below. 





