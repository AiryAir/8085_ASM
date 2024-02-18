# Program Statement
Write an ALP for division of two 8-bit numbers using bit rotationmethod. 
Dividend is present at memory location 5250H and divisor is present at memory location 5251H. 
Store the quotient at memory location 5252H and remainder at memory location 5253H.

# Program Logic
DIVIDEND/DIVISOR = QUOTIENT and REMAINDER
Inputs: Dividend, Divisor 
Output: Quotient, Remainder

We will have two cases here.

## CASE 1: When Divisor > Dividend 
Dividend will be subtracted from divisor. The total count of the number of subtractions will be the quotient till divisor becomes zero or dividend > divisor. 
When the latter happens, we move on to case 2.

## CASE 2: When Divisor < Dividend
The result of the subtraction is stored as remainder.

## Example

5/2 | Divisor > Dividend | Remainder = 3 | Quotient = 0; Q++; 1
1/2 | Divisor < Dividend | Remainder = 1 | Quotient = 0; Q++; 2

# Explanation

	\# ORG 2000H	 // tells the assembler where the program resides in the memory
	\# BEGIN 2000H   // begins code from the given memory location

		LHLD 2501	// Load data from given location to HL pair registers for dividend
		LDA 2501	// Load data for accumulator to get divisor from 2501
		STA 5250	// Store accumulator data in memory location 5250
		  LDA 2503	// Load data for accumulator from 2503
		STA 5251	// Store accumulator data in memory location 5251
		MOV B,A	// Move data between registers A and B
		MVI C,08	// move data “08” immediately to register C

	LOOP:       
	       DAD H     // double add data in H
	       MOV A,H   // move data from H to A
	       SUB B     // subtract B from accumulator
	       JC AHEAD  // conditional jump: jump if carry
	       MOV H,A   // move data from A to H
	       INR L     // increment by 1 in L
	
	AHEAD:       
	       DCR C	    // decrement value in C
	       JNZ LOOP	    // conditional jump: jump if not zero; jump to “LOOP”
	       SHLD 5252	// store data in HL Pair in memory location 5252
	       HLT		    // Halt/Terminate the program
	
	\# ORG 2501H		// Origin of data to be given to location 2501
	\# DB 9BH,00H,1AH	// LSB OF DIVIDEND, MSB OF DIVIDEND, DIVISOR
