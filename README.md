[![Build Status](https://travis-ci.org/nathan7/fn_box.svg?branch=master)](https://travis-ci.org/nathan7/fn_box)
# fn_box

  Box up your FnOnces!

## API

```rust
trait FnBox<Args, Result = ()> {
  extern "rust-call" fn call_box(self: Box<Self>, args: Args) -> Result;
}

impl<F, Args, Result> FnBox<Args, Result> for F where F: FnOnce<Args, Result> { … }

#[cfg(not(when_coherence_is_fixed))]
impl<'a> FnOnce() for Box<FnBox() + 'a> { … }

#[cfg(when_coherence_is_fixed)]
impl<'a, Args, Result> FnOnce<Args, Result> for Box<FnBox<Args, Result> + 'a> { … }
```

## Usage

```rust
let hello: Box<FnBox()> = Box::new(move || { println!("hello world!") });
hello();

let plus_one: Box<FnBox(_) -> _> = Box::new(move |x: i32| { x + 1 });
assert_eq!(plus_one.call_box((3,)), 4);
```
