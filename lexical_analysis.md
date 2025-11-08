### Author: @Override [see creds](#creds) 

# Lexical Analysis

Phase One: The ***First*** phase of the compiler is called the ***Lexical Analysis*** or ***Scanning***

> The Lexical Analysis reads the ***stream of characters*** making up the source program and grouping them into Lexemes (Or Terminals). For each Terminal, the Lexical Anaylizer produces a token: (ex)

```html
 <token-name, attribute-value>

```

- the first component `token-name` is an abstract symbol that is used during the syntax analysis
- The second component `attribute-value` points to an entry in the ***Symbol table*** for this token.

EX:
```
position = initial + rate * 60
```
Here is how the characters in this sequence would be parsed:

[1]: `position` is a Terminal (Lexeme) that would be mapped into `<id, 1>`, where ***id*** is an abstract symbol standing for ***identifier*** and 1 points to the ***symbol table*** entry for `position`. The symbol-table entry for an identifier holds information about the identifier, such as it's ***name*** and ***type***.

[2]: `=` is a Terminal (lexeme) that is mapped into the token `<=>`. Since this token needs no ***attribute-value***, we have omitted the second component. We could have used any abstract symbol such as `assign` for the token-name, but for notational convenience we have chosen to use the Terminal itself as the name of the abstract symbol.

[3]: `initial` is a Terminal (lexeme) that is mapped into the toekn `<id, 2>` where 2 points to the symbol-table entry for `initial`.

[4]: `+` is a Terminal (lexeme)that is mapped into the token `<+>`

[5]: `rate` is a Terminal (lexeme) that is mapped into the token `<id, 3>` where 3 points to the symbol-table entry for `rate`

[6]: `*` is a Terminal (lexeme) that is mapped into the token `<*>`

[7]: `60` is a Terminal (lexeme) that is mapped into the token `<number, 1>`

blanks seperating the Terminals are discarded by the lexical analyzer. Here the result after the lexical analyzer:
```
<id, 1> <=> <id, 2> <+> <id, 3> <*> <number, 1>
```

# Syntax Analysis

Phase Two: Also known as ***Parsing***, the parser uses the first components of the tokens produced to create a ***tree-like intermediate representation*** that depicts the grammatical structure of the token stream.

> Typical representation of a ***Syntax Tree*** (or ***Abstract Syntax Tree***) in which each interior node represents an operation and the children of the node represent the arguments of the operation. (We will Design one using a CFG Context-Free Grammar)

# Semantic Analysis



# creds
