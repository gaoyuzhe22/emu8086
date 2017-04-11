# emu8086 Assembler

the assembler is plan to assemble 8086 source code to binary code.

## Language Elements

 - label
 - expression
 - pesudo instruction

### Sentence format

```
[label:] [prefix] code [arguments] [;commentry]
[name] pesudo_code [arguments] [;commentry]
```

### pesudo code

 - `segment` and `ends`
 - `dx`
 - `equ` and `=`
 - `assume`
 - `org`
 - `proc` and `endp`
 - `times`

### expression prefix

 - `dword | word | byte`
 - `length`
 - `size`
 - `type`

### label info

 - label name
 - segment name and addr
 - code or data
 - length (if data)
 - type (if data)
 - size (if data)

### segment info

 - no different segment or more than one segments
 - bind to? `[CS, DS, ES, SS]`

## implementation

### segment

segment undefined:
 - only one block
 - `DS` same as `CS` to offset `0x00`
 - data can be define in code anywhere

segment defined:
 - at most 4 defined, called `Code, Data, Stack, Extra`
 - segment name need assume, assume code will find first, will assmeble as __segment undefined__ if don't exist
 - `data` segment has reference of only `data` segment
 - `stack` segment has reference of only `stack` segment
 - `code` segment
   - reference of data will refer to `data` segment and `stack` segment depend on memory reference of `BP` and other
   - code jmp and call will refer to `code` segment only
 - end of segment `ENDS` don't needed, but a warning will given if needed

### data element

`data element` refer to a parameter of one instruction, which can refer to a block of memory or specific register.

Properties:
 - where: memory or register
   - memory: base segment and EA address
   - register: name
 - length: register byte or word, memory byte, word or dword or *default*

Syntax:
 - register: `{name}`
 - memory: `{length}? {ptr}? {array}?[{expression}]`
 - immediate: `{expression}`

### procedure element

`proc element` refer to a program procedure label or name

###