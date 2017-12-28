# Building the ROP Chain

### By: Danny Colmenares 
#### Twitter: @malware_sec

#### Welcome back! 
This is the second part to our Smashing the Stack series. Please look over [Part 1](https://malwaresec.github.io/Stack-Based-Buffer-Overflow/) if you haven't already.

In Part 1 we focused on controlling the execution flow of our executable, it's really important that you fully understand how we developed that exploit because we will be using it in this writeup and add in our ROP chain to invoke our shell and get our flag (remember we're using the SECCON CTF [baby_stack](https://github.com/MalwareSec/Stack-Based-Buffer-Overflow/blob/master/baby_stack-7b078c99bb96de6e5efc2b3da485a9ae8a66fd702b7139baf072ec32175076d8.dms) challenge)

