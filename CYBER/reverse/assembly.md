# Base 
when create code that will run by the machine. the code is only binaries .
the bnaries read line by line in x{arch bit} bit line. (32 bit line for x32 or 64 bit line for x64).
but human can read binaries data we will use a more human readable format .

the format is line by line like the bin code each line have 3/4 parameter :

{instructions} param1, param2 --> will be tranlated in --> 1010101 01010 010101(64 or 32 bit format)

# Opcodes 
the instructions create a special number named opcode. this allow the cpu to know what to do .


# operandes

the operandes are the parameters there is multiple possiblity of operand :
immediate : constant value
register : name of the register directly in the assembly code will be translate in a special number that is the register
Memory operands : denotated by [], denote a memory location. we can use [(register name)] that will be translated to : the address stored in the registry.
