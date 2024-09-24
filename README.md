![{2AA5C6AD-30EF-4D1A-BCBE-E2304C6DA6EA}](https://github.com/user-attachments/assets/c217dd11-9160-42f9-8047-c786866f6ab9)Lab#1: Conduct buffer overflow attack on bof1.c, bof2.c, bof3.c programs.
![{8EF66160-18B3-4675-9D73-C58DEEAD9752}](https://github.com/user-attachments/assets/6650dca1-c40a-4c7a-80be-1249435086c3)
- started by changing directories, move into "Seclabs" folder and then the "buffer-overflow" folder
- run Docker with docker run, --privileged flag allows the container to access all devices on the host system, especially for  low-level system access may be required, -v flag mounts the local folder C:/Users/nguye/Seclabs to /home/seed/seclabs in the container, allows the container to work with my files
  ![{61924F50-5C35-4FF0-AA00-44DDCDCB0ABC}](https://github.com/user-attachments/assets/1f96198b-1e0e-4647-8394-f8213ad03374)
  ![{A4E02F71-F882-45B0-8894-57B353996921}](https://github.com/user-attachments/assets/6f939b9d-dd73-4c0b-822b-b3c3bf410ee5)
- Compile bof1.c,  warning arises because the gets() function is unsafe, it doesn't check the input size, lead to potential vulnerabilities
- Disable Stack Protection, gcc -w -g bof1.c -o bof1.out -fno-stack-protector -mpreferred-stack-boundary=2
disables the compiler's stack protection mechanisms, ensures compatibility with a 32-bit stack alignment
- Inspect secretFunc the instruction at 0x0804846b pushes the base pointer (ebp), and at 0x0804847d, it returns (ret)
- trigger the Buffer Overflow python -c "print('a'*204+'\x6b\x84\x04\x08')" generates a payload consisting of 204 'a' characters follow by the address 0x0804846b, starting address of secretFunc, format: \x6b\x84\x04\x0
- Congratulation! shows that the buffer overflow successfully altered the control flow
Segmentation fault the program attempts to continue, but the memory structure is corrupted

![{7E60E460-CF05-4A0C-9E9F-565960401398}](https://github.com/user-attachments/assets/0cc52c86-7709-4c7c-9d98-95d51d96e854)
- Compile bof2.c
- echo $(python -c "print('a'*40+'\x6b\x84\x04\x08')") | ./bof2.out 1234 creates a string of 40 'a' characters, '\x6b\x84\x04\x08' is the address of the function
- [buf]: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaak�♦  shows the contents of the buffer, 'a' characters have filled the buffer, followed by the memory address provided
[check] 0x804846b confirms that the value in the memory location being checked is 0x0804846b  is the address of the function that i want to jump to, confirms that my buffer overflow successfully overwrote the return address or function pointer, and the program is now set to jump to 0x0804846b
You are on the right way! show that the exploit worked correctly
![{91444646-FCA6-4B5F-8872-F882FDF40FDF}](https://github.com/user-attachments/assets/52b85d3f-5f83-4270-ba0c-8d04e0217828)
![{2AA5C6AD-30EF-4D1A-BCBE-E2304C6DA6EA}](https://github.com/user-attachments/assets/e507e9c9-7f3b-44a6-b487-f32fbb9a6df1)
- Compile bof3.c
- Launch GDB
-  Inspect the shell(), disassembled the shell(). function appears to call puts(), which prints a string located at address 0x8048540. It ends with a ret instruction, which returns control to the address stored on the stack
-  echo $(python -c "print('a'*128+'\x5b\x84\x04\x08')") | ./bof3.out creating the Exploit Payload. creates a string of 128 'a' characters, '\x5b\x84\x04\x08' is the memory address of the shell() function (0x0804845b)
-  You made it! The shell() function is executed  the buffer overflow successfully overwrote the return address, and the program executed the shell() function as intended, puts() inside shell() was designed to print










  


   
