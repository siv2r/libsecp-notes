# Implementation Details
1. All APIs available to users are present in `secp256k1.h`. It makes sense that there is no `secp256k1_fe_create` API to maximize security.
2. Use only needs API calls like ECDSA sign, verify etc.

# Questions
1. What is a context?
2. What is tweak? ex. schnorr sing tweak
3. Why can't we initialize a function pointer globally in C?
   Ex. let void (*funcPtr)(char *); be the func pointer declaration
   Now if we do `funcPtr = &greet` outside main function. We get error.
4. What is Opaque, transparent data structure? (in C)
5. What is the formula for ECDSA signature?
6. inline keyword (C/C++)
7. extern in C
8. what is SECP25K1_API macro? I don't understand.
9. will #include "test1.c" work inside a test2.c file? Can we access var declared in test1.c in test2.c in this case?
10. when we do `gcc test1.c test2.c` are global var in both (test1, test2) share same memory space?
11. Let's say we have file1.c, file2.c, file3.c and var1 is defined globally in file1.c. Will `extern int var1` in file2.c make the var1 accessable to file3.c? (during compilation)
12. Confirm if secp256k1 source files is compiled with -fvisibility=hidden
13. char vs unsigned char?
14. What is the tweak32 add to the seckey? in the `secp256k1_ec_seckey_tweak_add` api
15. Why does pubkey have both `tweak_add` and `tweak_mul`? pubkey is a vector with only one operation right?
   - does tweak_mul, multiply a scalar with the EC point vector?



