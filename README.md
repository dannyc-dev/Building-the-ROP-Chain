# Building the ROP Chain

### By: Danny Colmenares 
#### Twitter: @malware_sec

#### Welcome back! 
This is the second part to our Smashing the Stack series. In [Part 1](https://malwaresec.github.io/Stack-Based-Buffer-Overflow/) we focused on controlling execution flow and overwriting our return address using (**free and open source tools**): 

![alt text](screenshot/15.png)

It's important that we fully understand how we developed the exploit in Part 1 because we will be using it in this writeup and adding in our ROP chain to invoke our shell and get our flag (remember we're using the SECCON CTF [baby_stack](https://github.com/MalwareSec/Stack-Based-Buffer-Overflow/blob/master/baby_stack-7b078c99bb96de6e5efc2b3da485a9ae8a66fd702b7139baf072ec32175076d8.dms) challenge)

