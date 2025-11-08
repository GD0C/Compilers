# Compilers

### Credits

Shout out to the Compilers, Principles, Techniques and Tools book by Alfred V.



--------

```tex
\begin{document}


\title{Compilers}

First let's create a list of definitions: \\[0.5cm]

\textbf{Definition}: \\[01.cm]

\textbf{Syntax}: of a language is said to be a programming language can be defined by describing what it's program looks like\\[0.1cm]
\textbf{semantics}: The meaning of a program.\\[0.1cm]

\textbf{Context-free grammars}: Known as CFGs, are used for specifying the syntax of a language - one famous form of notation for a CFG is known as \textbf{Backus-Naur Form}\\[0.1cm]



\end{document}  

```

# 2 building a simple one-pass compiler

Breaking down a programming language into 2 ideas.

1) Describing what the program looks like is called the ***syntax*** of the language.

2) The meaning of the program is said to be the ***semantics*** of a language.

For specifying the syntax of a language, we use a ***Context-free grammar*** (CFG) - the most famous known as Backus Nuar From (BNF).

> Besides specifying the syntax of a language, a CFG can be used to help guide the translation of programs. A grammar-oriented compiling technique know as ***syntax-directed translation***, is very helpful for organizing a compiler front end.



Going to construct a simple compiler that translates infix expressions into postfix form. a notation in which the operators appear after their operands.


        Operator
           ↓
     X   +   Y
     ↑       ↑
    Operands



EX:

`9 - 5 + 2` ⇒ `9 5 - 2 +`
----------

in our compiler, the ***Lexical Analyzer*** converts the stream of input chars into a stream of tokens that becomes the input to the following phase... 

> (Note: the "Syntax-directed translator" is comprised: ***syntax analyzer*** ^ ***intermediate-code generator***)

<style>
.main-container {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 20px;
    padding: 10px 10px;
}
.char-stream {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(255, 0, 0, 0.2);
  width: 100px;
  height: 100px;
  text-align: center;
  border-radius: 10px;
}
.lexical-analyzer {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(186, 110, 86, 0.2);
  width: 100px;
  height: 100px;
  text-align: center;
  border-radius: 10px;
}
.token-stream {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(98, 215, 125, 0.2);
  width: 100px;
  height: 100px;
  text-align: center;
  border-radius: 10px;
}
.translator {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: cneter;
  background-color: rgba(210, 125, 255, 0.2);
  width: 100px;
  height: 100px;
  text-align: center;
  border-radius: 10px;
}
.ir {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(96, 215, 255, 0.2);
  width: 100px;
  height: 100px;
  border-radius: 10px;
}
.comparison {
    font-size: 15px;
    text-align: center;
    background-color: rgba(255, 255, 255, 0.2);
}
</style>

<div class="main-container">
    <div class="char-stream">
        Char stream 
    </div>
        ⇒
    <div class="lexical-analyzer">
        Lexical Anaylzer
    </div>
        ⇒
    <div class="token-stream">
        Token Stream
    </div>
        ⇒
    <div class="translator">
        Syntax-directed translator
    </div>
        ⇒
    <div class="ir">
        IR
    </div>
</div>


## 2.2 syntax def

We are going to use the CFG for the "specification" of the front end of the compiler.


> A ***Grammar*** naturally describes the hierarchical structure of many programming language constructs.

- Here is an `if-else` statement example from `C`:

```C
if (expression) statment else statement
```
 is the concatenation of: 
 <div class="comparison">
 keyword `if` || open parenthesis || `expression` || close parenthesis || `statement` || keyword `else` || `statement`
 </div>


Let's make things abstract:
`expr` = expression
`stmt` = statement
`->` = "can have the form"

We can write:

```
stmt -> if (expr) stmt else stmt
```

> Above is known as a ***production rule***

expr and stmt are ***non-terminals*** while `if`, `(`, `)`, `else` are ***terminals***.


### CFG

Context-free grammars are represented by:

- 1: T={ set of tokens, terminals }
- 2: N={ Set of non-terminals }
- 3: P={ set of productions where each production consists of a nonterminal, called the ***left-side*** of the production, an arrow, and a sequence of ***terminals*** and or ***non-terminals*** }
- 4: S={ Start symbol }


--------

our Grammar:
```
list -> list + digit
    | list - digit
    | digit
digit -> 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9
```

with this language, the starting non-terminal is `list` while the ending is `digit` and the terminals are `+`, `-`, `digit`, `0`, `1`, `2`, `3`, `4`, `5`, `6`, `7`, `8`, `9`.

-----

### Lexical Analysis

The first phase of the compiler is called the ***Lexical Analysis*** or ***Scanning***

> It's main task is to read the input characters and produce as output a sequence of tokens that the parser uses for syntax analysis.

The image below is commonly implemented by making the lexical analyzer be a subroutine or a coroutine fo the parser.

When getting the "next token" command from the parser, the lexical analyzer reads input characters until it identify the next token.

<style>
    .lexical-container {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 20px;
        padding: 10px 10px;
    }
    .lexical-diagram {
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: rgba(255, 0, 0, 0.2);
        width: 100px;
        height: 100px;
        text-align: center;
        border-radius: 10px;
    }
    .parser {
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: rgba(98, 217, 98, 0.2);
        width: 100px;
        height: 100px;
        text-align: center;
        border-radius: 10px;
    }
    .token {
        display: flex;
        flex-direction: column;
    }
    .lex-inner {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        gap: 20px;
        padding: 10px 10px;
    }

</style>

<div class="lexical-container">
    <div class="lex-inner">
        Source Program -> 
    </div>
    <div>
        <div class="lexical-diagram">
                Lexical Analyzer
        </div>
    </div>
    <div>
        <div class="token">
            <div>
                -> Token 
            </div>
            <- get next token
        </div>
        <-[Symbol Table]->
    </div>
    <div>
        <div class="parser">
            Parser
        </div>
    </div>
</div>
