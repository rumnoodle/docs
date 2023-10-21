# Rust Programming Language

## rustup

$ `rustup update` updates rust.

$ `rustup self uninstall` uninstalls rust.

$ `rustup doc` opens local documentation in browser.

## cargo

A lot of external libraries and tools can be found at crates.io, just add them as dependencies to Cargo.toml and use them in your projects.

$ `cargo new <project name>` creates a project with the name project name. With the `--vcs` option it is possible to set which, if any, version control software to use.

$ `cargo build` builds a cargo project. `--release` flag makes optimizations for release builds.

$ `cargo check` checks that it compiles but doesn't build it. Good when developing as it runs faster than build.

$ `cargo run` runs a cargo project and builds it if it hasn't been built.

$ `cargo update` updates the dependencies in Cargo.toml.

$ `cargo doc --open` opens documentation of project and dependencies in browser.

## Compiling

$ `rustc <filename.rs>` compiles a rust file.

## Formatting

$ `rustfmt <filename.rs>` formats a rust file to follow code conventions.

## References, Ownership, and Lifetimes

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

## Syntax

### Comments

`// This is a comment`

### Imports

```
use std::io; // a Rust library dependency
use rand::Rng; // an external dependency added in Cargo.toml
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
let mut s = String::from("Doh"); //create a mutable string
s.push_str(", says Homer"); // append a string literal to the newly created string
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

## Resources and References

* The Rust Programming Language ([https://doc.rust-lang.org/book/](https://doc.rust-lang.org/book/ "Link to the Rust Programming Language book"), retreived: 2023-10-21)
* [https://crates.io](https://crates.io "Link to crates.io, open source collection of crates"), a collection of open source crates to import and use in projects
