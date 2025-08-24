# Base  of assembly
see course assembly 
# instructions

## mov 

allow to mov data between memory location 

mov destination, source

mov eax, esb --> mov the data of eax in esb
mov eax, 0x5f --> put 0x5f in eax
mov eax, [0x5fc53e] --> put in eax what ther is memory at location 0x5fc53e

# nop 

do nothing put eax in eax. used for waiting cpu or for hacker that want to not the code will be th first displayed

# LOA

lea destination, source

load the memory address of the source in dest 

# shift 

shr destination, count
shl destination, count

allow to shift binarues values . add 0 in the end of the binaries values

# rotate 

ror destination, count
rol destination, count
same as shift but in the reverse order . bit comme from the left and right bits are removed 

# arithmetic 

# add 

value is added to the destination, and the result is stored in the destination.

add destination, value

# sub
value is sub to the destination, and the result is stored in the destination.
Zero Flag (ZF) is set if the result of the subtraction is zero.
if the destination is smaller than the subtracted value, then the Carry Flag (CF) is set.

sub destination, value

# mul
 always multiple the eax with the value give in parameter. the result is stored in 2 reg edx:eax
mul value


# div
 always divide the eax with the value give in parameter. the result is stored in 2 reg edx:eax
div value

# inc dec
 the value need to be a register and add 1 or dec 1 and store the value in the same register 
inc value
dec value

# and / or / not / xor

binari operation between dest and value the result is stored in the dest.  for not only the dest is needed and will be not

and dest, value
or dest, value
xor dest, value
not dest


# comparator

## test
The test instruction performs a bitwise AND operation, and instead of storing the result in the destination operand as the AND instruction does, it sets the Zero Flag (ZF) if the result is 0.

test destination, source


## CMP

the CMP instruction compares the two operands and sets the Zero Flag (ZF) or the Carry Flag (CF)
The Zero Flag (ZF) is set if both operands are equal. If the source operand is greater than the destination operand, the Carry Flag (CF) is set. it used sub instruction.


cmp destination, source

# jump

## jump

jump to the jump location . the EI will be modied 
jmp location

## jz 

jump only if the zero flag is set
jmp location

## jnz
jump only if the zero flag is not set

jnz location

## Used After CMP command

### je

Jump if equal. Often used after a CMP instruction.

### jne 

Jump if not equal. Often used after a CMP instruction.

### jg / jge

Jump if the destination is greater / greater equal than the source operand. Performs signed comparison and is often used after a CMP instruction.

### jl / jle

Jump if the destination is less / less equal than the source operand. Performs signed comparison and is often used after a CMP instruction.

### ja / jea

same as jg but not signed comparaison (not check sign)

### jb / jbe

same as jl but not signed comparaison (not check sign)


# stack instruction

## PUSH

push a reigster in the stack
push source

## POP
pop the last value in the stack in the register 
pop destination

## CALL

call a function that is in location (same as jump but with argument)

call location


