# Building the ROP Chain

### By: Danny Colmenares 
#### Twitter: @malware_sec

#### Welcome back! 
This is the second part to our Smashing the Stack series. In [Part 1](https://malwaresec.github.io/Stack-Based-Buffer-Overflow/) we focused on controlling execution flow and overwriting our return address using **free and open source tools**: 

![alt text](screenshot/15.png)

It's important that we fully understand how we developed the exploit in Part 1 because we will be using it in this writeup and adding in our ROP chain to invoke our shell and get our flag (remember we're using the SECCON CTF [baby_stack](https://github.com/MalwareSec/Stack-Based-Buffer-Overflow/blob/master/baby_stack-7b078c99bb96de6e5efc2b3da485a9ae8a66fd702b7139baf072ec32175076d8.dms) challenge)

This is how our exploit looks so far: 

![alt text](screenshot/14.png)

Just to recap, our first three lines are the offset we use for the padding to our printf functions where we pass an address and how many bytes to read after that address (we picked an arbitray 0x8 so it reads 8 bytes starting at 0xc82003bd60). We found those offsets using pattern_create and pattern_offset (or whichever techniques you feel most comfortable with). 

Our current payload looks like this:

```
paylod = 'A' * off_printf1 + p64(0xc82003bd60) + p64(0x8) + 'A' * off_printf2 + p64(0xc82003bd60) + p64(0x8) + 'A' * off_retaddress + p64(0xdeadbeef)
```
