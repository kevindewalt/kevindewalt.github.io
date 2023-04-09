## Supporting legacy applications written in Assembly language

In this particular use case we are trying to determine if a programmer who doesn't know Assembly language could use ChatGPT to maintain an application. Here are examples based on some [Assembly code for reversing a string(https://www.geeksforgeeks.org/8086-program-to-reverse-a-string/#)].

### Generating documentation

#### Example

Here is a README file ChatGPT can generate for the code.

# Assembly Code README

This Assembly program reverses a given string and prints the reversed string on the screen.

## Overview

The provided Assembly code consists of the following segments:

1. **.MODEL SMALL**: Defines the memory model to be used as 'SMALL'.
2. **.STACK 100H**: Sets the stack size to 100H (256 bytes).
3. **.DATA**: The data segment containing the string to be printed.
4. **.CODE**: The code segment containing the main procedure and the reverse function.

## Main Procedure

The main procedure (`MAIN PROC FAR`) is responsible for:

1. Loading the data segment.
2. Calling the reverse function to reverse the given string.
3. Loading the address of the reversed string and printing it on the screen.
4. Exiting the program.

## Reverse Function

The reverse function (`REVERSE PROC`) is responsible for:

1. Loading the offset of the string.
2. Counting the number of characters in the string.
3. Pushing each character onto the stack.
4. Reversing the string by popping the characters from the stack and placing them back into the string in reversed order.
5. Adding a '$' character at the end of the reversed string.

## Usage

To use this Assembly code, follow these steps:

1. Assemble and link the provided code.
2. Run the generated executable file.
3. Observe the reversed string printed on the screen.

#### Prompt
```
Generate a README for this Assembly Code. Show the final results as markdown code so I cut and paste it into a document called assembly.md.

---Assembly Code---
.MODEL SMALL
.STACK 100H
.DATA

; The string to be printed
STRING DB 'This is a sample string', '$'

.CODE
MAIN PROC FAR
MOV AX,@DATA
MOV DS,AX

; call reverse function
CALL REVERSE

; load address of the string
LEA DX,STRING

; output the string
; loaded in dx
MOV AH, 09H
INT 21H

; interrupt to exit
MOV AH, 4CH
INT 21H

MAIN ENDP
REVERSE PROC
	; load the offset of
	; the string
	MOV SI, OFFSET STRING

	; count of characters of the;
	;string
	MOV CX, 0H

	LOOP1:
	; compare if this is;
	;the last character
	MOV AX, [SI]
	CMP AL, '$'
	JE LABEL1

	; else push it in the;
	;stack
	PUSH [SI]

	; increment the pointer;
	;and count
	INC SI
	INC CX

	JMP LOOP1

	LABEL1:
	; again load the starting;
	;address of the string
	MOV SI, OFFSET STRING

		LOOP2:
		;if count not equal to zero
		CMP CX,0
		JE EXIT

		; pop the top of stack
		POP DX

		; make dh, 0
		XOR DH, DH

		; put the character of the;
		;reversed string
		MOV [SI], DX

		; increment si and;
		;decrement count
		INC SI
		DEC CX

		JMP LOOP2

				
	EXIT:
	; add $ to the end of string
	MOV [SI],'$ '
	RET
		
REVERSE ENDP
END MAIN

---End Assembly Code---
```