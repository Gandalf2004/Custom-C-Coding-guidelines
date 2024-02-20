# C Coding Style Guide

## Table of Contents
1. [Introduction](#1.-introduction)
2. [General Guidelines](#2.-general-guidelines)
3. [Formatting](#3.-formatting)
4. [Naming Conventions](#4.-naming-conventions)
5. [Comments](#5.-comments)
6. [Functions and Methods](#6.-functions-and-methods)
7. [Variables](#7.-variables)
8. [Control Structures](#8.-control-structures)
9. [Error Handling](#9.-error-handling)
10. [Documentation](#10.-documentation)
11. [Testing](#11.-testing)
12. [Miscellaneous](#12.-miscellaneous)



## 1. Introduction

### 1.1 Purpose
The purpose of this document is to define a consistant C coding style for all. Adhering to these guidelines will ensure consistency and readability throughout the codebase.



## 2. General Guidelines

### 2.1 Indentation
Use tabs for indentation. Configure your editor to change the number of spaces to 4.

### 2.2 Line Length
Limit lines to max 120 characters to maintain readability.
And if you aim for older systems or backwards compatibility, a max of 80 characters per line.

### 2.3 File Encoding
Use UTF-8 encoding for all source code files.

### 2.4 Version Control
Follow GitHub Flow for version control. Provide meaningful commit messages.



## 3. Formatting

### 3.1 Bracketes
Place opening Bracketes on the same line as the control statement and functions. For example:
``` C

if (condition) {
    // code
} else {
    // code
}
```


when you call functions and use arguments and parameters.
you should place a space after opening Bracketes and one before closing Bracketes.
```C

//# bad
int* dynamicInt = (int*)malloc(sizeof(int));
    if (dynamicInt == NULL) {
        // Allocation failed
        fprintf(stderr, "Memory allocation failed\n");
        exit(1); // Exit with an error code
    }

//# good
int* dynamicInt = (int*)malloc( sizeof( int ) );
    if ( dynamicInt == NULL ) {
        // Allocation failed
        fprintf( stderr, "Memory allocation failed\n" );
        exit(1); // Exit with an error code
    }
```

the same thing with conditions in IF statements or loops:
```C

//# bad
    if (dynamicInt == NULL) {
        // Allocation failed
        fprintf(stderr, "Memory allocation failed\n");
        exit(1); // Exit with an error code
    }


//# good
    if ( dynamicInt == NULL ) {
        // Allocation failed
        fprintf( stderr, "Memory allocation failed\n" );
        exit(1); // Exit with an error code
    }
```


### 3.2 Spacing
use a space before and after operators and assingments:
```C

// # Bad
if( (*prt)==NULL ){
    //...
}

// # Good
if( (*prt) == NULL ){
    //...
}

// # Bad assingment
int assingment=42;

// # Good assingment
int assingment = 42;
```

Use a single blank line to separate logical sections within a function.

### 3.3 Pointers in C and C++.
when you declarate a raw pionter in C and C++, put the asterisk beheind the type declaration ant not before the identifier.
```C++

// # Bad
int *myPrt = &x;

// # Good
int* myPrt2 = &x;
```

Place Bracketes around Dereferenced raw pointers. For better visibility of Dereferenced pointers in the code. For example:
```C

// # Bad
  
*dynamicInt = 42;
printf( "%d", *dynamicInt );

// # Good

(*dynamicInt) = 42;
printf( "%d", (*dynamicInt) );
```


In C++, if you are using C-style pionters (raw pointer) use the Brackete notation `(*prt).method();` to avoid confusion . But if possibly use C++ pointers (unique pointer or shared pointer) and use the arrow notation `prt->method();`.
```C++

// # Bad
void HttpPage(WiFiClient* client){

  *client.print( "Click <a href=\"/TEST\">here</a> to switch to AmpelTest.<br>" );
  *client.print( "Click <a href=\"/R\">here</a> to switch to Red signal.<br>" );
  *client.print( "Click <a href=\"/RY\">here</a> to switch to Yellow, Red signal.<br>" );
  *client.print( "Click <a href=\"/Y\">here</a> to switch to Yellow signal.<br>" );
  *client.print( "Click <a href=\"/G\">here</a> to switch to Green signal.<br>" );
}

// # Good
void HttpPage(WiFiClient* client){

  (*client).print( "Click <a href=\"/TEST\">here</a> to switch to AmpelTest.<br>" );
  (*client).print( "Click <a href=\"/R\">here</a> to switch to Red signal.<br>" );
  (*client).print( "Click <a href=\"/RY\">here</a> to switch to Yellow, Red signal.<br>" );
  (*client).print( "Click <a href=\"/Y\">here</a> to switch to Yellow signal.<br>" );
  (*client).print( "Click <a href=\"/G\">here</a> to switch to Green signal.<br>" );
}

// # much Better
void HttpPage(std::unique_ptr<WiFiClient> client) {

    client->print("Click <a href=\"/TEST\">here</a> to switch to AmpelTest.<br>");
    client->print("Click <a href=\"/R\">here</a> to switch to Red signal.<br>");
    client->print("Click <a href=\"/RY\">here</a> to switch to Yellow, Red signal.<br>");
    client->print("Click <a href=\"/Y\">here</a> to switch to Yellow signal.<br>");
    client->print("Click <a href=\"/G\">here</a> to switch to Green signal.<br>");
}
```

If you Dereference a raw pointer from a struct in C and C++, use the arrow notation. For example:
```C

// # Bad
uint32_t xorshift128(struct xorshift128_state* state) {
	
	uint32_t s = (*state).x[0];  
	(*state).x[3] = (*state).x[2];
	(*state).x[2] = (*state).x[1];
	(*state).x[1] = s;
}

// # Good
uint32_t xorshift128(struct xorshift128_state* state) {
	
	uint32_t s = state->x[0];  
	state->x[3] = state->x[2];
	state->x[2] = state->x[1];
	state->x[1] = s;
}
```



## 4. Naming Conventions
Be consistent: Use the same naming conventions throughout your code.

### 4.1 Variables
Use descriptive and meaningful variable names. to reduce the need for comments in your code.

Use camelCase for variables.
``` C
int exampleVar = 42;
```
Use meaningful names: Choose names that reflect the purpose or content of the variable. 
Avoid single-letter variable names unless they are widely accepted conventions (e.g., i for loop counters).

Avoid abbreviations (unless widely accepted): Clear names are better than saving a few keystrokes. However,
common abbreviations like num for "number" are generally acceptable.

### 4.2 Constants
Use uppercase and Snake_Case for constants and macros names.
```C

#define LED_PIN 26

const char* SSID_FALLBACK = "ITTN";
```

### 4.3 Functions/Methods
Use descriptive and meaningful function names. to reduce the need for comments in your code.

Use camelCase for function names.
```C

void ManageClientRequest();
```


Follow a verb-noun pattern: Name functions based on actions they perform.
Use verbs to indicate operations and nouns to describe the object or result.
```C

// # Bad
int performOperation();

// # Good
int calculateSum();
```

Be specific: Clearly convey what the function does. A reader should have a good idea of the function's purpose
without looking at its implementation.
```C

// # Bad
int* processData();

// # Good
int* cleanAndNormalizeData();
```

Avoid side effects: A function should ideally do one thing. 
If a function has a side effect, make it clear in the name or consider refactoring.
```C

// # Bad
void updateData();

// # Good
void saveDataToDB();
```

Include docstrings for functions in Headers using the Doxygen-Coding style.



## 5. Comments

### 5.1 Inline Comments
Use inline comments sparingly. Comments should explain "why," not "what."
```C

// # Bad
x = x + 1  // Increment x by 1

// # Good
x = x + 1  // Compensate for the off-by-one error in the loop
```


## 5.2 Docstrings

Include docstrings for functions in Headers using the Doxygen-Coding style.
```C++

/**
 * @brief Reads a line from a WiFiClient connection and returns it as a string.
*
* @param client A pointer to the client (as WiFiClient*).
* 
* @details This function reads characters from the client connection one by one.
* If it encounters a newline character ('\n'), it checks if the next character is a carriage return character ('\r').
* If it is, it reads and discards it. The function then returns the line that has been read so far.
* If the read character is not a newline, it is appended to the line being read.
* If there are no more characters to read from the client connection, the function returns the line that has been read
 so far.
* 
* @return A string (as an Arduino String class not cpp) that contains the current line read from the client connection
*. The String does not contain the ending newline character.
 */
```


## 6. Functions and Methods

### 6.1 Function Length
Keep functions and methods short and focused. Aim for larger Functions 60 to 80 lines of code.
Consider breaking down larger functions into smaller, more modular ones to improve readability and maintainability.

### 6.2 Return Statements
Have a single return statement per function. except if you return error codes or exceptions.



## 7. Variables

### 7.1 Variable Initialization
Try to Initialize variables at the point of declaration if possible.
```C

// # Bad
int myNum;

// # Good
int myNum2 = 0;
```



## 8. Control Structures

### 8.1 If Statements
Avoid nested if statements when possible and do not nest more than 5 times. If you have to nest more than 5 times consider to restructure your code into helper-functions.
```Java

// # Bad example
public static double root(int degree, double radicand) {
    double x = 1.0;
    int iterations = 100;
    
    if (degree < 0) {
        return 0.0;
        
    } else {
        if(degree %2 == 0 && radicand < 0.0) {
            return 0.0;
            
        } else if(degree %2 == 0 && radicand > 0.0) {
            for(int i = 0; i < iterations; i++) {
                x = x - (Mathe.pow(x, degree) - radicand) / (degree * Mathe.pow(x, degree -1));
            }
            return x;
            
        } else {
            if (radicand < 0.0) {
                
                radicand = radicand *-1.0;
                for(int i = 0; i < iterations; i++) {
                    x = x - (Mathe.pow(x, degree) - radicand) / (degree * Mathe.pow(x, degree -1));
                }
                return x *-1.0;
                
            } else {
                for(int i = 0; i < iterations; i++) {
                    x = x - (Mathe.pow(x, degree) - radicand) / (degree * Mathe.pow(x, degree -1));
                }
                return x;
            }
        }
    }
}
```
```Java

// # Better
private static double calculateRootViaNewtonsMethod(int degree, double radicand){

    double x = 1.0;
    int iterations = 100;
    for(int i = 0; i < iterations; i++) {
        x = x - (Mathe.pow(x, degree) - radicand) / (degree * Mathe.pow(x, degree - 1));
    }
    return x;
}

private static double calculateRoot(int degree, double radicand){

    if(degree %2 == 0 && radicand < 0.0) {
        return Double.NaN;

    } else if(degree %2 == 0 && radicand > 0.0) {
        return calculateRootViaNewtonsMethod(degree, radicand);

    } else {

        if (radicand < 0.0) {

            radicand = radicand *-1.0;
            return calculateRootViaNewtonsMethod(degree, radicand) *-1.0;

        } else {

            return calculateRootViaNewtonsMethod(degree, radicand);
        }
    }
}

public static double root(int degree, double radicand) {

    if (degree < 0) {
        return Double.NaN;

    } else {
        return calculateRoot(degree, radicand);
    }
}
```



## 9. Error Handling

### 9.1 Check Return Values
Many C functions return -1 or NULL, or set an error code as a global variable, in case of an error. You can check these return values to handle errors. For example, you can check if `malloc()` returns NULL to handle memory allocation errors
```C

#include <stdio.h>

int main() {
   char *ptr = malloc( 2000000000UL );
    if ( ptr == NULL ) {
        perror( "malloc failed" );
        exit(-1);
    }
   
   return 0;
}
```

### 9.2 Use the `errno` Variable
The `errno` variable is accessible by programs after including `<errno.h>`. This variable indexes error descriptions accessible by the function `strerror(errno)`. You can use `strerror(errno)` to print a detailed error message 
```C

FILE* fp = fopen("seggs.txt", "r");
if (fp == NULL) {
  printf("Value of errno: %d\n", errno);
  printf("The error message is : %s\n", strerror(errno));
}
```

### 9.3 Error Callbacks
For more modular and clean error handling, especially in larger applications or libraries, error callbacks can be utilized. They allow specific functions to be called when an error occurs.
```C

void onError( const char* message ) {
  printf( "Error: %s\n", message );
}

int compute( int x, int y, void (*callback)(const char*) ) {
  if ( y == 0 ) {
      callback( "Division by zero." );
      return -1;
  }
  return x / y;
}

int main() {
  int result = compute( 10, 0, onError );
  if ( result == -1 ) {
      printf( "Computation failed.\n" );
  } else {
      printf( "Result: %d\n", result );
  }
  return 0;
}
```

If you have custom error codes consider making an `enum` for your errors:
```C

enum CustonError{
    SUCSESS,
    DIVISION_BY_ZERO = -1,
    SPECIFIC_ERROR2  = -2,
}

void onError( const char* message ) {
  printf( "Error: %s\n", message );
}

int compute( int x, int y, void (*callback)(const char*), enum CustonError* error ) {
  if ( y == 0 ) {
      callback( "Division by zero." );
      (*error) = DIVISION_BY_ZERO;
      return -1;
  }
  (*error) = SUCSESS;
  return x / y;
}

int main() {
 enum CustonError error;

 int result = compute( 10, 0, onError, &error );
 if ( error == DIVISION_BY_ZERO ) {
     printf( "Division by zero error.\n" );

 } else if ( error == SPECIFIC_ERROR2 ) {
     printf( "Specific error 2.\n" );

 } else {
     printf( "Result: %d\n", result );

 }

 return 0;
}
```

### 9.4 Common C Error Codes
The C language has a range of predefined error codes stored in the `errno.h` header file. Some of these include `ENOMEM`, `EIO`, and `EINVAL`. While each error code is associated with a specific issue, it's essential to refer to the documentation or use the `perror` function for detailed information.
```C

#include <errno.h>
#include <string.h>

void checkError(int result) {
  if (result == -1) {
      printf("Error: %s\n", strerror(errno));
  }
}
```


## 10. Documentation

### 10.1 Code Documentation
Document complex code blocks, algorithms, and non-trivial decisions.

### 10.2 README
Maintain an up-to-date README file with installation instructions, usage guidelines, and project details.
