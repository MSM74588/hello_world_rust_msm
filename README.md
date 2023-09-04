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

- `loop{}` will create an infinite loop 
