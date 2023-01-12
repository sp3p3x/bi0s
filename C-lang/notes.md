# **C**

## Loops:

  - while:

    syntax:

    ```c
    While(condition)
    {
    //statements inside the loop
    }
    ```

    example:

    ```c
    #include <stdio.h>
    
    int main() {
      while (1) {
        printf("hello world\n");
      }
      return 0;
    }
    ```

  - for:

    syntax:

    ```c
    for( initialization statement; condition)
    {
    //statements inside the loop
    }
    ```

    example:

    ```c
    #include <stdio.h>

    int main() {

      for (int i = 0; i < 10; i++) {
        printf("hello world\n");
      }

      return 0;
    }
    ```

  - do while:

    syntax:

    ```c
    do
    {
    //statements inside the loop
    }
    While(condition);
    ```

    example:

    ```c
    #include <stdio.h>

    int main() {

      do {
        printf("hello world\n");
      } while (1);

      return 0;
    }
    ```

## If:

  - If - else:

    syntax:

    ```c
    If(condition)
    {
    Statement(s);
    }
    else
    {
    Statement(s)
    }
    Statement
    ```

    example:

    ```c
    #include <stdio.h>

    int main(){
      if (1<2){
        printf("1 is less than 2\n");
      }
      else{
        printf("1 is not less than 2\n");
      }
      
      return 0;
    }
    ```

  - nested if:

    syntax:

    ```c
    If(condition)
    {
    If(condition)
    {
    Statement(s);
    }
    Else
    {
    Statement(s)
    }
    }
    ```

    example:

    ```c
    #include <stdio.h>

    int main(){
      if (3<4){
        if (1<2){
          printf("1 is less than 2\n");
        }
        else{
          printf("1 is not less than 2\n");
        }
      }
        return 0;
    }
    ```

  - else if:

    syntax:

    ```c
    If(condition)
    {
    Statement(s);
    }
    else if(condition)
    {
    Statement(s)
    }
    …
    Else
    {
    Statement(s)
    }
    ```

    example:

    ```c
    #include <stdio.h>

    int main(){
      if (1<2){
        printf("1 is less than 2\n");
      }
      else if(2<4){
        printf("2 is less than 4\n");
      }
      else{
        printf("1 is not less than 2\n");
      }
      
      return 0;
    }
    ```

## Switch:

  - switch case:

    syntax:

    ```c
    Switch(expression)
    {
    Case label1:
    Statement(S);
    Break;
    Case label2:
    Statement(S);
    Break;
    ...
    Case labelN:
    Statement(s);
    Break;
    Default:
    Statement(s);
    Break;
    }
    ```

    example:

    ```c
    #include <stdio.h>

    int main(){
      int output = 0;

      switch (output) {
        case 0:
          printf("weee\n");
          break;
        case 1:
          printf("brrr\n");
          break;
        default:
          printf("boop\n");
          break;
      }

      return 0;
    }
    ```

## Conditional operator:

  - conditional:

    syntax:

    ```c
    (condition)? expr1: expr2
    ```

    example:

    ```c
    #include <stdio.h>

    int main(){
      (1<2)?printf("1 is less than 2"):printf("1 is not less than 2");

      return 0;
    }
    ```

## goto:

  - goto statement:

    syntax:

    ```c
    goto labelname;
    labelname;
    ```

    example:

    ```c
    #include <stdio.h>

    int main(){
      printf("Hello world!\n");
      goto here;
      printf("this wont get executed");

      here:
      printf("this will get executed");

      return 0;
    }
    ```

## Functions:

  - Creating a new function:

    syntax:

    ```
    returnType functionName (parameters){
      logic
    }
    ```

    example:

    ```c
    void foobar() {
      printf("Hello world");
    }
    ```

  - Calling a function:

    syntax:

    ```
    functionName(arguments);
    ```

    example:

    ```c
    int main() {
      foobar(); // function call
    }
    ```

## Pointers:

  - Declaration:

    syntax:

    ```c
    type *pName;
    ```
  
    example:

    ```c
    int *foobar;
    ```
  
  - Assignation:

    syntax:

    ```c
    pName = &variableName;
    ```

    example:

    ```c
    foobar = &marks;
    ```

## Structures:

  ### Structures can be used to group items of different types.

  - Declaration:

      syntax:

      ```c
      struct structTag {
        dataType varName; // each variables are called members
        dataType varName;
        .
        .
      }
      ```

      example:

      ```c
      struct school {
        int studentNumber;
        int staffNumber;
        char grade;
      }
      ```

  - Assignation:

    syntax:

      ```c
      struct structTag structName;

      structName.variable1=value;
      structName.variable2=value;
      ```
    
    example:

      ```c
      struct school school1;

      school1.studentNumber = 400;
      school1.staffNumber = 200;
      school1.grade = 'B';
      ```

## Objects:

  - Reference:

    - https://www.quora.com/What-is-an-object-in-C
    - https://stackoverflow.com/questions/40441612/difference-between-variable-and-data-object-in-c-language
    - https://port70.net/~nsz/c/c11/n1570.html#3.15
    - https://www.geeksforgeeks.org/differences-between-procedural-and-object-oriented-programming/

## File I/O:

- ### 'FILE' pointer type is used

  - syntax:
    ```c
    FILE *filePointer;
    ```

- ### fopen() in stdio.h is used to open a file
  - syntax:
    ```c
    filePointer = fopen("filepath","mode");
    ```

- ### fclose() in stdio.h is used to close a file
  -syntax:
    ```c
    fclose(filePointer);
    ```

  ### Ref:
  - https://www.programiz.com/c-programming/c-file-input-output

## Recursion:

 - ### Example: To find the factorial of a number using recursion

    ```c
    #include<stdio.h>

    long int fact(int n) {
        if (n>=1)
            return n*fact(n-1);
        else
            return 1;
    }

    int main() {
        int n=10;
        printf("Factorial = %ld", fact(n));
        return 0;
    }
    ```


<br>

# **Ref**

- https://wiki.bi0s.in/reversing/c-lang/
- https://www.quora.com/What-is-an-object-in-C
- https://stackoverflow.com/questions/40441612/difference-between-variable-and-data-object-in-c-language
- https://port70.net/~nsz/c/c11/n1570.html#3.15
- https://www.geeksforgeeks.org/differences-between-procedural-and-object-oriented-programming/
- https://www.educba.com/control-statements-in-c/
- https://www.programiz.com/c-programming/c-file-input-output 