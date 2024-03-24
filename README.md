### Tasks

Solutions and instructions to reproduce of provided tasks.

## Task 1 (log4j vulnerability that allows execution of remote code)

Exploiting Log4Shell/CVE-2021-44228 vulnerability based on app provided, "vulnapp.jar"


















# Task 2

The challenge involved a binary with a vulnerability in the way it handled heap-allocated variables. The objective was to manipulate the heap to overwrite a specific variable (`safe_var`) with the string "ACMR". Upon success, the program would display the contents of "flag.txt".

## Environment Setup

Operating system: Ubuntu 22.04.3 LTS

## Approach and Solution

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

## Result

Using the payload `XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXACMR`, the heap overflow was successful, and the flag `acm_ftw` was printed as expected.

## Tools Used

- Decompilation software (e.g., Ghidra, IDA Pro) for analyzing the binary.
- A text editor to craft the payload.
- Terminal to interact with the binary program.

## Lessons Learned

This challenge was an excellent demonstration of a heap overflow vulnerability and underscored the importance of bounds checking in programs. It also showed the power of manual analysis in understanding and manipulating memory in a controlled manner.

![image](https://github.com/theg1239/tasks/assets/52027622/5d86b751-baee-4716-b0d1-3908840c4d9a)










