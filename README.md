<p align="center">
  <a href="" rel="noopener">
 <img height=300px src="https://i.imgur.com/5FcT7si.png" alt="Project logo"></a>
</p>

<h3 align="center">RED Programming Language</h3>

<div align="center">
    
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](/LICENSE)

</div>

---

## Installation

NOTE: A new update has simplified built-in libraries as they will auto-install when you need them. A version control system will be added soon as well

The easiest way to get started with RED is to download this whole repository. If you dont want the examples you can delete those but make sure to keep built-in as this contains built-in libraries that will also be updated over time. Next you have to build the interpreter written in Golang 1.19:

```bash
go build run.go
```

A binary called run should pop up and you can use this to run your red files! On MacOS the command to run red files using this is:

```bash
./run path-to-red-file.red
```

Wheras on windows it will be:

```bash
run.exe path-to-red-file.red
```

And voila! It should work

To create a binary using RED code you have to use the compiler and to do that first run:

```bash
go build compile.go
```

A binary called compile should pop up and you can use this to compile your red files! On MacOS the command to compile red files using this is:

```bash
./compile path-to-red-file.red name-for-binary
```

Wheras on windows it will be:

```bash
compile.exe path-to-red-file.red name-for-binary
```

Then you can run the new binary using ./name-for-binary or name-for-binary.exe for MacOS and Windows respectively. For now you will need the built-in folder in the root directory of where you run your binary or red file from but this may be resolved in an update. If you don't have it a new system has been implemented where RED will automatically try to install built-in modules to where you are running the file from by cloning the built-in. This works well but it is still suggested to do it yourself!

Do not alter the built-in folder's name, the interpreter or the compiler's code unless you really know what you are doing and if you do modify the interpreter or the compiler's code make sure to rebuild the binary. Report any bugs here on github please.

There will be no automatic updating system so if you wish to update your red just redownload the repo and follow the above instruction

## Usage

The language is fundamentally quite simple as it is stack-based. This means that it functions based on an array/stack containing stackValue's with just 3 main datatypes which are:

- Number (can be integer or float64 based on usecase) (id: 0)
- String (id: 1)
- Boolean (id: 2)

There are also arrays with datatype id 4 but they are difficult to work with. If you are planning to work with them I would suggest familiarising yourself with the language first and then creating a keyword library to simplify their use. Also, who knows, I may create a built-in library to help.

The base keywords (not including built-in util library and module keywords) are:
- PUSH (pushes a value to the front of the stack)
- STORE (removes the top value from the stack and stores it into a symbol/variable)
- LOAD (load the value of a symbol/variable to the top of the stack)
    - LOAD can also get elements from an array
- Comparatives (compares top 2 elements, E means and equal)
    - GT
    - GTE
    - LT
    - LTE
    - EQ
    - NOT (flips boolean on top of stack)
- Operators (self-explanatory in operation; take 2 top numbers from stack and add them)
    - ADD
    - SUB
    - DIV
    - MULT
- Trig ratios (all in radians and the ones prefixed with A are inverse) (they work on last number in stack and replace it)
    - SIN
    - COS
    - TAN
    - ASIN
    - ACOS
    - ATAN
- Other math stuff (they work on last number in stack and replace it)
    - LOG, log base 10
    - LN, natural log
    - SQRT, square root
- Array stuff
    - MAKEARRAY (clears stack and stores whole stack in array, then puts this array into stack)
    - JOIN (joins array from of stack of strings on top of stack)
    - SPLIT (splits string using delimiter into array)
    - REMOVE
    - SETINDEX (first argument is the array, 2nd is the index and it will set that index to the last thing in the stack)
    - APPEND
- Datatype conversion (from top of stack)
    - FLOAT (conversion to number, can also be integer, input must be string)
    - BOOL (conversion to bool, input must be string)
    - STR (converts anything to string)
- Misc
    - IMPORT (imports .mred module file, 2nd argument defines the reference word)
    - KEYPORT (imports .kr module file containing keywords)
    - STRCAT (concatencate top 2 strings on stack)
    - DELAYST (takes last number from stack and delays that many milliseconds)
    - INPUT (takes user input till new line and then puts string on top of stack)
    - IF (condition variable) (command) (takes boolean variable and if true does command in rest of args) (eg IF higher UITL PRINT "higher")

You can also define functions but cannot define functions in them or call functions inside them:
- FUNC (starts function definition and will continue till ENDFUNC keyword is found)
- ENDFUNC (ends write of functions)
- RUN (can run a function but also introduces loop functionality as the second (optional) argument can be while condition which will keep the function running)

In modules or .mred files the only keywords that can be used are:
- EXPORT (exports a variable to an importing file so the variable can be changed)
- EXARR (same functionality as export but for arrays)
- FUNC & ENDFUNC (defines module functions which can be ran with MODRUN)
    - In functions all the regular keywords can be used

There are more keywords when it comes to using these modules in regular files like:
- MODSTORE (changes an exported variables)
- MODGET (gets value of module symbol)
- MODRUN (runs a module function)

Finally more default keywords are provided by the util library to simplify your life like (all of these must be prefaced with "UTIL "):
- INITNUM, INITBOOL, INITSTR all take 1 argument, and assign a default value (0, true or "" based on datatype) to the symbol, which unlike in STORE has to be written inside quotes, in the first argument (eg. INITNUM x will make a variable called x assigned 0)
- SET sets the second argument as the value for the first symbol. If datatype is mismatched it simply wont work
- PRINT which differs from the default PRINT by printing an argument
- PRINTVAR will print a symbol

Again note that in all keyword lib keywords, symbols must be referred to as strings.

Finally there are a few predefined variables that may be expanded on representing mathematical constants. To get them simply LOAD them as with any library. It is highly recommended not to reassign them as libraries may use them:
- PI (gives approximate value for pi)
- EULER (gives approximate value for constant e)

Seperate from this, I highly recommend you to check out all the different examples to see most of these keywords in use!

Here is an example of a simple single-file powering function where num represents the number being powered and power represents the degree (power>=2). Feel free to change these around and test on your own computer!

```python
UTIL INITBOOL "powering"
UTIL INITNUM "num"
UTIL INITNUM "power"
UTIL INITNUM "powc"

UTIL SET "num" 2
UTIL SET "power" 3
UTIL SET "powc" 2

LOAD num
STORE current

FUNC power
    LOAD current
    LOAD num
    MULT
    STORE current
    LOAD powc
    PUSH 1
    ADD
    STORE powc
    LOAD powc
    LOAD power
    GTE
    STORE powering
ENDFUNC

RUN power powering

UTIL PRINTVAR "current"
```

Another example using some more keywords is of a number guessing game:

```python
RANDINT 0 100
STORE num

UTIL INITBOOL "guessed"
PUSH "I have picked a number between 0 and 100. Can you guess it?"
PRINT

PUSH "Enter your guess: "
PRINT 

FUNC guess
    INPUT
    FLOAT
    STORE guess

    LOAD num
    LOAD guess
    LT
    STORE LT
    
    LOAD num
    LOAD guess
    GT
    STORE GT

    LOAD num
    LOAD guess
    EQ
    STORE EQ

    IF EQ UTIL PRINT "Correct!"
    IF LT UTIL PRINT "Higher!"
    IF GT UTIL PRINT "Lower!"

    LOAD EQ
    NOT
    STORE guessed
    
    UTIL PRINT ""
    PUSH "Enter your guess: "
    IF guessed PRINT
ENDFUNC

RUN guess guessed
```

Please share your creations under issues with the creation label!

## Contributing

Pull requests are welcome. For major changes, please open an issue first
to discuss what you would like to change.

Considering the state this was developed in, there will likely be bugs and if there are please do report them on github.

## License

[MIT](https://choosealicense.com/licenses/mit/)
