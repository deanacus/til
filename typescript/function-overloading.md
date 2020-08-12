# Function Overloading

I occasionally find myself wanting to have a function that will take multiple
argument signatures, and do slightly different things based on that signature.

This is a concept known as function overloading. As someone relatively new to
TypeScript, I was stuck for a little while as to the best way to define these
function types. I could never seem to get multiple function definitions working
(never mind the fact that I favour function expressions over function
definitions), and instead use an interface with multiple signatures:

```
interface OverloadedFunction {
  (signature1Arg) => string;
  (signature1Arg, Signature2Arg2) => string;
  ...
}

const functionToOverload: OverloadedFunction = (...args) => {
  // Implementation goes here
}
```

Might not be the most correct way of doing things, but it works for me currently.