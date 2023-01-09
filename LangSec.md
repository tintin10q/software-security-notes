Langsec is pointing out problems with input language and how people deal with them

## Shotgun parsing 

Devs take scattershot approach to parsing data in bits and pieces mixing recognition and processing. You should only process when you have the entire thing parsed.

## Weird machine

A buggy parser provides weird behaviour and that provides a strange execution platform which might but Turing complete. Apparently you can have turning complete by using page faults and double page faults, so you never touch the CPU, what. 

## Principles to prevent buggy parsing

1. Precisely defined input languages
2. Generated parser code
3. Complete parsing before processing
4. Keep the input language simple & clear
5. Make the parsing require not a lot of power limited 

The parser should be an inverse of the pretty printers (unparsers). 

