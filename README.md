# Homework 2 (20 Points)

Please submit your solution via NYU Classes.

The deadline for Homework 2 is September 21, 11:55pm.

To obtain a local copy of this repository. Open a terminal at the
location where you want the repository to reside and execute the
command

```bash
git clone https://github.com/nyu-pl-fa18/hw02.git
```

## Problem 1 (4 Points)

Consider the following pseudo code.

```modular-3
 1: procedure main () {
 2:   var a: Int := 1
 3:   var b: Int := 2

 4:   procedure middle () {
 5:     var b: Int := a

 6:     procedure inner () {
 7:       println(a); println(b)
 8:       var a: Int := 3
 9:     }
       
10:     var a: Int := 3

11:     inner()
12:     println(a); println(b)
13:   }

14:   middle()
15:   println(a); println(b)
16: }
```

Suppose this was code for a language with the declaration-order rules
of C (but with nested subroutines) - that is, names must be declared
before use, and the scope of a name extends from its declaration
through the end of the block. At each print statement, indicate which
declarations of `a` and `b` are in scope. What does the program print
(or will the compiler identify static semantic errors)? Repeat the
exercise for the declaration-order rules of C# (names must be declared
before use, but the scope of a name is the entire block in which it is
declared).

## Problem 2 (4 Points)

Consider the following pseudo code:

```pascal
var x: Int // global variable

procedure set_x(n: Int) {
  x := n
}

procedure print_x () {
  println(x)
}

procedure first () {
  set_x(1)
  print_x()
}

procedure second () {
  var x: Int
  set x(2)
  print_x()
}

procedure main () {
  set_x(0)
  first()
  print_x()
  second()
  print_x()
}
```

What does this program print if the language uses static scoping? What does
it print with dynamic scoping? Why?


## Problem 3 (6 Points)

Consider the following fragment of code in C:

```c
{
  int a, b, c;
  ...
  {
    int d, e;
    ...
    {
      int f;
      ...
    }
    ...
  }
  ...
  {
    int g, h, i;
    ...
  }
  ...
}
```

a) Assume that each integer variable occupies 4 bytes and that the
   shown variable declarations are the only variable declarations
   contained in this code. How much total space is required for the
   variables in this code when the program is executed?

b) Describe an algorithm that a compiler could use to assign stack
   frame offsets to the variables of arbitrary nested blocks, in a way
   that minimizes the total space required.

## Problem 4 (6 Points)

As part of the development team at MumbleTech.com, Janet has written a
list manipulation library for C that contains, among other things, the
following code

```c
typedef struct list_node {
  void* data;
  struct list_node* next;
} 

list_node;
list_node* insert(void* d, list_node* L) {
  list_node* t = (list_node*) malloc(sizeof(list_node));
  t->data = d;
  t->next = L;
  return t;
}

list_node* reverse(list_node* L) {
  list_node* rtn = 0;
  while (L) {
    rtn = insert(L->data, rtn);
    L = L->next;
  }
  return rtn;
}

void delete_list(list_node* L) {
  while (L) {
    list_node* t = L;
    L = L->next;
    free(t->data);
    free(t);
  }
}
```

a) Accustomed to Java, new team member Brad includes the following
   code in the main loop of his program:

   ```c
   list_node* L = 0;

   while (more_widgets()) {
     L = insert(next_widget(), L);
   }
   
   L = reverse(L);
   ```

   Sadly, after running for a while, Brad's program always runs out of
   memory and crashes. Explain what's going wrong.

b) After Janet patiently explains the problem to him, Brad gives it another
   try:

   ```c
   list_node* L = 0;

   while (more_widgets()) {
     L = insert(next_widget(), L);
   }
   
   list_node* T = reverse(L);
   
   delete_list(L);
   
   L = T;
   ```
   
   This seems to solve the insufficient memory problem, but where the
   program used to produce correct results (before running out of
   memory), now its output is strangely corrupted, and Brad goes back
   to Janet for advice. What will she tell him this time?
