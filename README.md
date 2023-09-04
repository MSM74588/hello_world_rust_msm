# LEARN RUST

September 4, 2023

## New Project:

- For **VS Code** use extension `rust-analyzer`, and for ".toml" use `Even Better TOML`

```console
cargo new hello_cargo
```

it create a package called hello_cargo

- it uses Git as VCS by default

- it created src > main.rs and Cargo.toml

- it also initialised git and added .gitignore

- TOML contains details of the project(package) and its package dependencies which we can use with cargo and is managed by cargo itself

- In Rust, packages of code are referred to as "crates"

- `cargo.toml`is basically the **Manifest** of the app

---

## Basic Code

- fn main () {}=> function

- ";" used

- println() is print function
  
  ---

## Building

- ```console
  cargo build
  ```

- this command creates an executable file  target/debug/hello_cargo.

- Because the default build is a debug build, Cargo puts the binary in
  a directory named *debug*.
  
  ---

## Running the executable

- ```console
  $ ./target/debug/hello_cargo # or .\target\debug\hello_cargo.exe on Windows
  Hello, world!
  ```
  
  - it can also be run with `cargo run`=> this compiles and runs it directly (not needing to run `cargo build` command)
  
  - `cargo check` => this checks the code to make sure it compiles, but does not compile it
  
  - `cargo check` much faster than `cargo build`
  
  ### Building for release
  
  - When ready to release, ``cargo build --release``  => to compile it with optimisation (removes the `debug` flag) [and created executabe in `target/release` instead of `target/debug` ]
  
  ---

## When `Git clone` other rust project

- to compile it just run `cargo build`, it will coordinte the *dependencies* ,  and then you can compile by `cargo build` or `cargo build --release` 

---

# Coding Rust

## Stage 1

- `use std::io` is used to *input from user*, basic I/O. this snippet imports the library and uses it, its the standard library, input-output library.

- `fn main()` is the default entry point into a program, currently `()` signifies there are no <u>parameter</u> 

- `println!()` is a macro, that print-line, prints a string

- ```rust
  let mut guess = String::new();
  ```
  
  - we use `let` to create <u>variables,</u>, here named as **guess**
  
  - By default, varaiables in rust are *immutable* (value won't change), to make it mutable we must add `mut`
  
  - `//` is comments in rust, everything after it is ignored
  
  - `=` sign tells rust to bind the value to it. It is now bound to a "string" due to `String::new`, a function that return  a new instance of string
  
  - the `new` is a function, that creates a new, empty string. 
  
  - You’ll find a `new` function on many types because it’s a
    common name for a function that makes a new value of some kind.
  
  - In full, the `let mut guess = String::new();` line has created a mutable
    variable that is currently bound to a new, empty instance of a `String`. Whew!

- ```rust
      io::stdin()
          .read_line(&mut guess)
  ```
  
  - now we call `stdin` fn we imported (or we could have also imported directly using `std::io::stdin`)
  
  - it returns an instance of `std::io::stdin`. now we will call `read_line` method of it, we are passing the inserted value to *guess*, the read_line function take the input as standard io and append that into the string, without overwriting its content. so we therefore pass the string as argument.
    
    - we are passing `&mut guess` as an argument to `read_line` basically
    
    - the `&` indicates that this argument (mut guess) is a **reference**, it allows us to access one peice of data without needing to copy that data into memory multiple times
    
    - we use `&mut guess` instead of `&guess` to make it mutable.

- ```rust
  .expect("Failed to read line");
  ```
  
  - **NOW we will handle errors (error handling)**
  
  - `io::stdin().read_line(&mut guess).expect("Failed to read line");`can alse be written like this
  
  - the `read_line` method returns an **enum** as "result", which is a type that can be in one of multiple possible state, each state is called a varient.
    
    - The variants are `Ok` and `err`, Ok means it successfully generated value, Err means operation failed and also contains information about the error
    
    - `expect` will cause the program to crash and display the msg we pa,ssed here, "Failed to read line", and if its Ok it will return the *value* that ok is holding. and we can then use the value. if expect is not called we will get a warning but the program will compile.
    
    - to supress  warning, we need to write error-handline code (that's the right way)

#### println!()

- placeholder => `{}` ex: `println!(something value: {variable})

- we can also use expression i placeholder: `println!("x = {x} and y  + 2 = {}", y + 2)`, this is comma seperated value.

#### Generating Random No.

- Now we will use `rand`package(crate) to generate a random number

- NOTE: to read pkg docs run `cargo doc --open`

- we can use `cargo add rand` or manully add `rand = "0.8.5"` to toml to add the rand crate/pkg

- `let secret_number = rand::thread_rng().gen_range(1..=100);`
  
  - we import rand and its trait `Random Number Generator`= `rng`by `use rand::Rng`it is also known as "trait". now it is added to scope.
    
    - first we call `rand::thread_rng`function, that gives us the particular random number generator we are going to use (one that is local to current thread of execution and is seeded by the OS), the we call `gen_range`method on RNG, this is defined by `Rng`trait. we brought in scope with the `use rand::Rng`. The `gen_range`method takes a range expression as an argument and generates a random number in the <u>range</u>, the range exp we are using here is `1..=100`to request a number b/w 1 and 100

#### Now comparing the guess to the Secret Number (by RNG)

- now we will use another standard library, cmp (comparing)

- ```rust
  use std::cmp::Ordering;
  ```
  
  - we are bringing a type called `std::cmp::Ordering` into scope from standard library. The `Ordering` type is another <u>enum</u> which has varient *Less, Greater and Equal* . These are the three outcomes that are possible when comparing two numbers (cmp)

- ```rust
  fn main() {
      // --snip--
  
      println!("You guessed: {guess}");
  
      match guess.cmp(&secret_number) {
          Ordering::Less => println!("Too small!"),
          Ordering::Greater => println!("Too big!"),
          Ordering::Equal => println!("You win!"),
      }
  }
  ```
  
  - Now we will use the `match` expression, (function is also an expression btw), to use the `guess.cmp`method and pass the "secret_number" value which is referenced. Which is a enum, and will return the valur of secret_number, when matched with the specific one.
  
  - The `match` expression is made up of arms, each arm consists of a pattern to match agains, tust takes the value given to match and looks through each arms pattern in turn. Patterns and match construct are powerful rust features.
  
  - here, the cmp method will reuturn the matched enum return value, if the secret no is 38 and the guess is 50, it will **compare the guess to secret number** > `guess.cmp(&secret_number)`
  
  - and when matched with the enum return value, it will print that command
  
  ### But this will not compile yet.
  
  - The code has mismatched types. since we wrote `let mut guess = String::new()`rust was able to infer "Guess" should be a string, but secret number is a i32 (32 but number type) [u32 is unsigned 32 bit number, i64 is 64 but number], so there is cearly a mismatch type, so we need to read the input as real number instead of String (as its returned), so we need to convert it. [this is because the input is taken as raw string with the arguments]
  
  - convert by 
    
    ```rust
    let mut guess = String::new();
    
        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");
    
        let guess: u32 = guess.trim().parse().expect("Please type a number!");
    
        println!("You guessed: {guess}");
    
        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => println!("You win!"),
        }
    ```
  
  - in which we after taking the input from i/o we the converted to u32, we shadowed the previous guess variable and reused the guess variable, shadowing lets us reuse the *guess* var rather than using two different unique var. Now we have converted the value
    
    - the `guess`referes to original var, the `trim`method on *String* instance will eliminate whitespace at the beginning and end, which is needed to comparison. as only numerical data is needed. We also added a `expect` so it will be able to handle unmatched type and handle the error.
    
    - the `parse`method on string, converts it to another type, here `u32`, u32 is good for small positive number.
    
    - the `parse` will only work on characted which can be logically cnverted to numbers, so it cal easily cause error, for example an emoji will not be converted to number and will retrun a `result`type that wll have the Err varient in this case

#### Now Looping it

- `loop{}` will create an infinite loop , `break;`statement will break the loop

#### Now Handling Invalid input

- currently the program just crashes

- so we will handle error by 

```rust
let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };
```

- when it is ok. the result (named as num) is passed to Ok varient as parameter so the enum return the value, and on error, `(_)`the underscore is a <u>catchall</u> value. We are using a `match`, so on Ok match we will return the "Ok" and the value to <u>guess</u> and on error will run `continue`which tells the program to run the next iteration of the loop skipping the current. so on error it will skip the loop and will again start/next iterate, and wil ask for new value again.

### Stage 2: Common Programming Concepts

- Mutability: You cannot change the value of a var later by default. like
  
  ```rust
  // # READ THE COMMENTS
  
  // ---
  let variable = 1
  println!("var is {varariable}")
  // and then
  variable = 2 // ❌
  // This is not allowed! its not safe
  // we NEED to declare the variable mutable
  // ---
  
  // ---
  // Correct way => added mut
  let mut variable = 1
  println!("var is {varariable}")
  // and then
  variable = 2
  println!("var is {varariable}")
  // note, we did not shadowed the previous variable, we just changed it
  // to shadow it we have to use let
  // ---
  ```

- Constants: `const`, its constant, mutability cannot be used
  
  - it can be used with expressions, example
    
    ```rust
    const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3
    ```
  
  - For constant: the naming convention is "UPPERCASE with UNDERSCORE in b/w" 
  
  - It can be easy to read, like instead of setting the valur to 10,800

- Scopes: we can create scopes with `{}` inside a code/function

- Shadowing + scope:
  
  ```rust
  fn main() {
      let x = 5;
  
      let x = x + 1;
  
      {
          let x = x * 2;
          println!("The value of x in the inner scope is: {x}");
      }
  
      println!("The value of x is: {x}");
  }
  ```
  
  - The program first binds "x" to 5, then it create a new variable by `let x =`whuch overtakes the original value by adding 1 as instructed. inside the scope, it overtakes the modified x and then create a new x by multiplying it by 2. and it is valid inside that scope only, the shadowing in that scope ends.
  
  - Shadowing is different from making a variable `mut`, we will get a compiler error if we do not use `let` keyword
  
  - We can change the type of a variable with shadowing:
    
    ```rust
        let spaces = "   ";
        let spaces = spaces.len();
    
        // THis will give us an error as the types are different
        // To correct this,  we must use shadowing
        // or change the type by parsing it maybe.
        let mut spaces = "   ";
        spaces = spaces.len();
    ```

- **Scalar Types**:

- Integer types:

- | Length  | Signed  | Unsigned |
  | ------- | ------- | -------- |
  | 8-bit   | `i8`    | `u8`     |
  | 16-bit  | `i16`   | `u16`    |
  | 32-bit  | `i32`   | `u32`    |
  | 64-bit  | `i64`   | `u64`    |
  | 128-bit | `i128`  | `u128`   |
  | arch    | `isize` | `usize`  |

- Each signed variant can store numbers from -(2n - 1) to 2n -
  1 - 1 inclusive, where *n* is the number of bits that variant uses. So an `i8` can store numbers from -(27) to 27 - 1, which equals
  -128 to 127. Unsigned variants can store numbers from 0 to 2n - 1,
  so a `u8` can store numbers from 0 to 28 - 1, which equals 0 to 255.

- Additionally, the `isize` and `usize` types depend on the architecture of the
  computer your program is running on, which is denoted in the table as “arch”:
  64 bits if you’re on a 64-bit architecture and 32 bits if you’re on a 32-bit
  architecture.

- Integer literal
  
  - | Number literals  | Example       |
    | ---------------- | ------------- |
    | Decimal          | `98_222`      |
    | Hex              | `0xff`        |
    | Octal            | `0o77`        |
    | Binary           | `0b1111_0000` |
    | Byte (`u8` only) | `b'A'`        |
    
    - can be written in any of this form, We can use Visual seperator like
      `1_000`instead of `1000`
    
    - We can also directly mention type, like for example u8 75 we can write as `75u8`
    
    - If unsure, generally use i32, it defaults. `isize` or `usize` is used in situation like when idexing some sort of collectin
    
    - in case of <u>integer Overflow:</u> Rust Panics and exists, when! using --release flag it will NOT check for panic so the variable will not be as expected, it is considered an error. 
      
      - To explicitly handle the possibility of overflow, you can use these families
        of methods provided by the standard library for primitive numeric types:
        
        - Wrap in all modes with the `wrapping_*` methods, such as `wrapping_add`.
        - Return the `None` value if there is overflow with the `checked_*` methods.
        - Return the value and a boolean indicating whether there was overflow with
          the `overflowing_*` methods.
        - Saturate at the value’s minimum or maximum values with the `saturating_*` methods.

- Floating point types:
  
  - `f32` and `f64` , default is f64 on modern cpu, all floating-points types are signed

- Numeric Operation:

```rust
fn main() {
    // addition
    let sum = 5 + 10;

    // subtraction
    let difference = 95.5 - 4.3;

    // multiplication
    let product = 4 * 30;

    // division
    let quotient = 56.7 / 32.2;
    let truncated = -5 / 3; // Results in -1

    // remainder
    let remainder = 43 % 5;
}
```

- Bool Type: `bool`:-> can be `false`or `true`, can be explicit with type annotation

- Char: `char`for type annotation

- Tuple Type [compound types]

- 
