# Rust Programming Language

## rustup

$ `rustup update` updates rust.

$ `rustup self uninstall` uninstalls rust.

$ `rustup doc` opens local documentation in browser.

## cargo

A lot of external libraries and tools can be found at crates.io, just add them as dependencies to Cargo.toml and use them in your projects.

$ `cargo new <project name>` creates a project with the name project name. With the `--vcs` option it is possible to set which, if any, version control software to use. `--lib` creates a library crate.

$ `cargo build` builds a cargo project. `--release` flag makes optimizations for release builds.

$ `cargo check` checks that it compiles but doesn't build it. Good when developing as it runs faster than build.

$ `cargo run` runs a cargo project and builds it if it hasn't been built.

$ `cargo update` updates the dependencies in Cargo.toml.

$ `cargo doc --open` opens documentation of project and dependencies in browser.

$ `cargo test` runs all tests in a project. `--show-output` shows output of successful test runs, if code has println! calls or calls that creates output.

$ `cargo test funs_to_test` runs all tests that have `funs_to_test` in the function name.

$ `cargo test --option-to-test-command -- --option-to-test-binary-after-double-dash`

$ `cargo test -- --ignored` runs all tests annotated with `#[ignored]`. Also possible to run all tests with `--include-ignored`.

$ `cargo test --test file_name_test` runs tests in the integration test file named `file_name_test.rs` in the tests directory.

## Compiling

$ `rustc <filename.rs>` compiles a rust file.

## Formatting

$ `rustfmt <filename.rs>` formats a rust file to follow code conventions.

## Syntax

### References, Ownership, and Lifetimes

Transferral of ownership happens with all values allocated on the heap.

```
let s1 = String::from("Whoa");
let s2 = s1; // ownership of the string is transferred from s1 to s1 here.
function_call(s1); // same thing would happen when calling a function, ownership is transferred to the function.
// This happens with return values as well, ownership is transferred to the calling code.

// s1 can not be used after ownership of the string has been transferred from it, trying to use it here would cause a compile time error

let s3 = s2.clone(); // this makes a clone of s2 for s3 to use

// s2 is still available here

let x = 5;
let y = x;

// both values still available as they are copied, they are on the stack
```

It is also possible to borrow values with references. Then the ownership is not transferred but it's still possible to use and alter the value if it's a mutable reference.

```
let s = String::from("Woowoo");
let len = calculate_length(&s); // we use a reference to s instead of the value (the & makes it a reference)

fn calculate_length(s: &String) -> usize { // note that we receive a reference, must be annotated
    s.len()
}
```

If we wanted to be able to alter the string in the calculate\_length function above we'd have to pass it as a mutable string `calculate_length(s: &mut String)`. It must be called like this `calculate_length(&mut s)`.

It's only possible to have one mutable reference to the same variable at a time. A reference is "active" until its last use.

It is possible to have more than one immutable reference but if you have immutable references it's not possible to have mutable ones to the same variable for as long as the immutable ones are alive, if there is a mutable reference it must be the only one.

### Comments

`// This is a comment`

### Tests

```
#[cfg(test)]
mod tests {
    use super::*; // imports all code from the outer module to be used in the tests

    #[test]
    fn test_fun() {}

    #[test]
    fn test_fun_two() {
        panic!("Tests fail if test function panics");
    }

    #[test]
    #[should_panic(expected = "Panic text should contain this string")] // test that function that is tested panics
    fn test_fun_three() {}

    #[test]
    fn test_fun_four() -> Result<(), String> { // it's also possible to return a Result to indicate success
        if test_success {
            Ok(())
        } else {
            Err(String::from("This test failed.")
        }
    }

    #[test]
    #[ignore] // ignores this test
    fn test_fun_five() {}
}
```

Tests that return `Result` are convenient for instance by making it possible to use the `?` operator.

Assertions that can be used in tests include `assert!`, `assert_eq!`, `assert_ne!`. Marcros testing for equality requires that what you're testing implements the `PartialEq` and `Debug` traits.

It's possible to pass a format string to assert functions along with parameters if required after the required parameters.

#### Integration Tests

Integration tests are written in the tests folder in the root of the crate if they exist.

```
use crate_name;

#[test]
fn test_function() {
    crate_name::some_fun();
}
```

They are run with all other tests when you execute `cargo test`.

Files in subdirectories in the tests folder are not compiled as separate crates and are thus not run as tests.

Only lib.rs files are possible to test with integration tests. It's not possible to import main.rs and test it.

### Modules

```
// src/lib.rs
pub mod moduleName;

pub use subModuleName; // re-exports a submodule from a module making use of the submodule easier as imports can use this shorter path (though not in this case but in deeply nested module structures this can be useful.)

// src/moduleName.rs or src/moduleName/mod.rs
pub mod subModuleName;

pub struct StructName {
    pub val: String, // values within structs need to be public as well
    val2: String, // if there's a private value the module needs a public constructor for constructing the struct
}; // a public struct in the module

impl StructName {
    pub fn new(val_one: &str) -> StructName {
        StructName {
            val: String::from(val_one),
            val2: String::from("Woowoo"),
        }
    }
}

// Enums and it's values are public as long as the enum is declared public
pub enum SomeEnum {
    ValOne,
    ValTwo,
    ValThree,
}

// src/moduleName/subModuleName.rs or src/moduleName/subModuleName/mod.rs
pub fn some_function() {} // pub keyword makes it public

fn some_other_function() {} // private function

// src/main.rs
use crate::moduleName::subModuleName;

pub mod moduleName; // is this needed?

subModuleName::some_function();
```

#### Imports

```
use std::io; // a Rust library dependency
use rand::Rng; // an external dependency added in Cargo.toml
use std::{cmp::Ordering, io}; // use of nested ordering
use std::io::{self, Write}; // use self to import module
```

### Variables and Constants

```
let variable_name = "value"; // creates an immutable variable
let mut variable_name = "value"; // creates a mutable variable
const CONSTANT_ZERO: i32 = 33 * 3 - 99; // creates a constant with the value of the result from the calculation
{
    let variable_name = 42; // shadows the first assignment of variable_name until the end of the block
}
```

### Strings and Characters

```
let mut s = String::new(); // create an empty mutable string
let mut s = String::from("Doh"); //create a mutable string, same as writing "Doh".to_string()
s.push_str(", says Homer"); // append a string literal to the newly created string
s.push('c'); // append a single character
s.as_str(); // converts a String into a &str

new_string = first_string + &second_string; // concatenates 2 strings, moves first_string so unusable after
new_string = format!({first_string}{second_string}"); // another way to write the above without taking ownership

for c in "...".chars() { // there is also a bytes() method
    // loop over a string, returning a character at a time
}
```

#### Slices and Ranges

```
let s = String::from("what is on the telly?"):
let slice = &s[1..10]; // creates the string slice "hat is on"
```

`..12` means a range from the beginning to (but not including) the 12th character and consequently `12..` means from the 12th character to the end of the string. `s[..]` is a slice of the entire string.

Be careful with slices though as they must be sliced on character boundaries, splitting a slice in the middle of a unicode character will cause the program to panic.

A slice has the `&str` type as does the string literal `let s = "This is a string literal"`.

`fn function_name(s: &str) ...` allows you to pass both `&str` and `&String` references.

Slices work with arrays as well.

#### println!(), format!() ...

Placeholders in strings: `format!("Replace {this} with the contents of variable named this.");`. They must be used within a formatting makro like println! or format!.

#### char

`let c: char = 'A'; // a character 4 bytes in size, a unicode scalar value U+0000 - U+D7FF, U+E000 - U+10FFFF`

### Numbers

```
wrapping_add(...); // wraps integer addition if overflow
checked_add(...); // returns None if overflow when adding two numbers
overflowing_add(...); // returns value and boolean for whether or not there was overflow
saturating_add(...); // returns a saturated value if overlow
```

### Structs

```
struct StructName {
    value_one: bool,
    value_two: String,
    value_three: String,
    value_four: i32,
}

let sct = StructName {
    value_two: String::from("Woowoo"),
    value_one: false,
    value_three: String::from("Lots of strings in this struct"),
    value_four: 123,
};

sct.value_two; // value is "Woowoo"
sct.value_two = "Doh"; // would work if sct was mutable, doesn't help if only value_two is mutable

fn create_struct(value_one: bool, value_two: String, value_four: i32) -> StructName {
    StructName {
        value_one, // use parameter passed to function, shorthand for value_one: value_one
        value_two,
        value_three: String::from("This is always the default value."),
        value_four,
    }
}

let sct2 = StructName {
    value_one: true,
    ..sct // shorthand for using the remaining values from sct
};
// sct is unusable after this as the data is moved to sct2 because sct contains values that are moved to sct2.
// Had we created new values for all movable data the other values would have been copied and sct still possible to use.
```

#### Methods and Associated Functions

```
impl StructName {
    fn some_method(&self, stringval: String) -> String {
        format!("{}{}", stringval, self.value_three)
    }
}
```

A method always takes self as the first parameter, `self` is short for `self: &Self`. We only borrow self, hence the reference. It is possible for methods to both borrow and take ownership, accept both mutable and immutable values.

It is also possible to create an associated function that isn't a method by omitting the self first parameter. `String::from` is one and is called using two semicolons instead of a dot.

```
impl StructName {
    fn new(trees: i32) -> Self {
        Self {
            ...
        }
    }
}
```

It's possible to have more than one impl block.

#### Tuple and Unit Structs

A tuple struct is a tuple with a name e.g. `struct StructName(i32, String)`.

A unit struct is a struct that doesn't have any attributes, mostly used to attach behavior to e.g. `struct StructName;`.

#### Derived traits

```
#[derive(Debug,AnotherTrait)]
struct Bee {
    weight: i32,
    wingspan: i32,
}

let bee = Bee { weight: 12, wingspan: 16 };
println!("{:?}", bee); // derived debug allows printing out debug info like this {:?} or {:#?} for formatted output
dbg!(&bee); // another way to print and debug
```

### Generics, Traits and Lifetimes

```
fn some_fun<T>(val: T) -> T { ... }

struct StructName<T> {
    one: T,
    two: T,
} // both one and two must have same type, to have different generic types use 2 generic type params

impl<T> StructName<T> {
    fn some_fun(&self) -> &T { ... }
}

impl StructName<String> {
    fn some_fun(&self) -> String { ... }
}

struct StructName<T, U> {
    one: T,
    two: U,
}

enum SomeEnum<T> {
    One(T),
    Two,
}
```

#### Traits

```
pub trait TraitName {
    fn trait_fn_one(&self) -> i32; // also possible to implement default methods with method bodies in traits
    fn trait_fn_two(&self, i32) -> i32;
}

impl TraitName for StructName {
    fn trait_fn_one(&self) -> i32 { ... }
}

pub fn function_name(item: &impl TraitName) { ... } // to implement a function that accepts a trait as parameter
pub fn function_name<T: TraitName>(item: &T) { ... } // previous line is syntactic sugar for this
pub fn function_name(item: &(impl TraitName + AnotherTrait) ... // to specify multiple traits
pub fn some_function<T, U>(t: &T, u: &U) -> String
where
    T: TraitName,
    U: TraitName + AnotherTrait,
{
    ...
}
pub fn some_function() -> impl TraitName { ... } // to return a value that implements a trait

impl<T: TraitName> StructName<T> { // implement only for types that have trait TraitName
    fn some_fun() ...
}

impl<T: TraitName> SomeTrait for T { ... } // implements SomeTrait for types that implement TraitName
```

#### Lifetimes

Every reference has a lifetime.

Lifetime annotation in the following function tells the compiler that the return value will live for as long as the parameter passed to the function which lives the shortest amount of time.

In the struct it says that the value in the struct needs to live for at least as long as the struct.

```
fn some_fun<'a>(x: &'a str, y: &'a str) -> &'a str {
    if some_cond {
        x
    } else {
        y
    }
}

struct StructName<'a> {
    val: &'a str,
}

impl<'a> StructName<'a> {
    fn some_method(&self, some_str: &str) -> &str { // lifetimes not necessary here because lifetime elision rules
        println!("{}", some_str);
        self.val
    }
}
```

`'static` lifetimes live for as long as the program lives.

### Enums

```
enum EnumName {
    First,
    Second(i32, i32),
    Fourth(String),
    Fifth { x: i32, y: i32 },
}

impl EnumName {
    fn some_method(&self) {
        ...
    }
}

let first = EnumName::First;

match value {
    EnumName::First => ... ,
    EnumName::Fourth(strval) => {
        ... // use strval here
    },
    _ => ... , // catchall, drop value. If using value name the catchall variable something e.g. other
    ...
}

if let EnumName::Fourth(strval) = first {
    // do something with strval here only if first is an EnumName::Fourth, ignore otherwise
}
```

### Tuples

```
let tup: (i32, char, bool) = (123, 'g', true);
let (x, y, z) = tup; // x = 123, y = 'g', z = true
let int_value = tup.0;
let unit = (); // an empty tuple is called a unit, returned by default from functions if there is no explicit return value
```

### Arrays

```
let arr: [i32; 4] = [1, 2, 3, 4]; // array type annotation [type of elements; length of array]
let arr = [22; 7]; // array with 7 elements, all set to 22
arr[0]; // first array element
```

Program panics if it tries to access an element outside the bounds of the array.

It is possible to create slices of arrays, see the "Slices and Ranges" section in Strings.

### Vectors

```
let v: Vec<i32> = Vec::new();
let v = vec![1, 2, 3]; // creates a vector with i32 values (default for numbers)

let mut v2 = vec![1, 2, 3];
v2.push(4);
let val: &i32 = &v2[1]; //gets second element of vector, panics if out of range
let val2: Option<&i32> = v2.get(1); // gets second element of vector, does not panic if out of range

match val2 {
    Some(value) => ... , // do something with value &i32
    None => ... , // do something when there's no value
}
```

The borrow checker works with vectors as well. All values accessed from the vector need to be borrowed or they will be lost to the vector. It is also not possible to mutate a vector where you have borrowed values from it and they are still actice, their lifetimes have not expired.

#### Iterating through a vector

```
let v = vec![1, 2, 5];
for i in &v {
    println!("{i}");
}

let mut v2 = vec![1, 3, 5];
for i in &mut v2 {
    *i += 1; // we have to dereference i here to get to the value of the reference rather than the reference
}
```

It's not possible to modify the vector in the for loop, only the values within.

Storing different values in a vector can be done with an enum.

```
enum SomeEnum {
    ValOne(i32),
    ValTwo(String),
}

let v = vec![
    ValOne(123),
    ValTwo(String::from("Woowoo"),
];
```

### Hash Map

```
use std::collections::HashMap;

let mut hm = HashMap::new();

hm.insert(String::from("Homer"), 0); // will replace the value for key Homer if it already exists
hm.entry(String::from("Homer")).or_insert(0); // inserts only if Homer doesn't exist
let homer = hm.entry(String::from("Homer")).or_insert(0); // fetches Homer or creates if it doesn't exist
*homer += 1; // adds one to the value of Homer
hm.get("Homer").copied().unwrap_or(-1);

for (key, value) in &hm {
    // iterate over a hashmap
}
```

HashMap returns an `Option<&V>` and the copied makes it a value rather than a reference.

Types that implement the `Copy` trait will be copied into the hash map, other values will be moved and can not be used afterwards.

References won't be moved but they must be alive for at least as long as the hash map is alive.

### Functions

```
fn function_name(one: i32, two: i32, three: bool) -> i32 { // parameters and return values must be annotated
    if three {
        return 0; // possible to return early with an explicit return statement
    }

    one + two // expressions don't have semicolons to end lines, ; turns it into a statement
}
```

Statements don't return anything. To return something from a function the last line must be an expression, then the result of calculating that expression is returned.

### Conditionals

```
if value >= 12 { // the condition must evaluate to a bool or code won't compile
    value - 1
} else if value <= 6 {
    value + 1
} else {
    value
}
```

`else if` and `else` are optional. Note that the values inside the blocks are expressions. Also notice the lack of parentheses in the condition.

`let num = if true { 12 } else { 11 };` possible to assign the result of an if expresion to a variable.

```
let res = match some_value {
    Ok(res) => {
        // do something with res
    },
    Err(err) => {
        // do something with err
    },
}
```

### Iterating and Looping

#### for

```
for element in arr {
    println!("{element}");
}

for index in (1..10).rev() { // loop through a range that has been reversed to simulate a countdown
    println!("{index}");
}
```

#### loop

```
loop {
    println!("Will loop forever unless there's a `break;` statement.");
}

loop {
    if value < 5 {
        continue; // starts next iteration of the loop without executing the rest of the code within the loop block.
    }

    println!("Will not print this if value < 5");

    if value > 99 {
        break value; // possible to return a value out of the loop, maybe caught with let v = loop { ...
    }
}

'outer_loop: loop {
    'inner_loop:  loop {
        break 'outer_loop; // will break out of both loops, break out to the label specified
    }
}
```

#### while

```
while true {
    println!("Effectively the same as loop but it is possible to test anything.");
}
```

### Errors

```
panic!("Will cause the program to panic");

// Recoverable errors can be handled with Result enum from std library
enum Result<T, E> {
    Ok(T),
    Err(E),
}

io::ErrorKind; // has a bunch of errors that can be used

fn some_fun() Result<i32, io::Error> {
    let val = some.method_call(&mut var)?; // the question mark says return Error if Error, set result to val if Ok
}
```
Check out the `From` trait to handle propagating a different type of error than the one in the return `Result`.

### Annotations

`#[cfg(some_config)]` means only add following item if a certain configuration option is set.

## Resources and References

* The Rust Programming Language ([https://doc.rust-lang.org/book/](https://doc.rust-lang.org/book/ "Link to the Rust Programming Language book"), retreived: 2023-10-21)
* [https://crates.io](https://crates.io "Link to crates.io, open source collection of crates"), a collection of open source crates to import and use in projects
* [https://doc.rust-lang.org/nomicon/](the Rustonomicon "the Rustonomicon"), retrieved: 2023-10-28
* [https://rust-lang.github.io/api-guidelines/](Rust API Guidelines "Guidelines on how to structure Rust modules and programs"), retrieved 2023-10-28
