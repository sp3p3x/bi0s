# **Reverse Engineering**

## **Notes**:
- **Static analysis**: In this approach, we do not execute the executable. Instead, we use specialized tools and prior knowledge to infer several useful information about the executable.
- **Dynamic analysis**: In this approach, we execute the executable and observe its behaviour during execution, using a different set of specialized tools. By observing its behaviour, we can infer useful information about the executable.

<br>

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