#### error

#### TS2304: cannot find name require and process
>https://github.com/DefinitelyTyped/DefinitelyTyped/issues/15329

```
Don't think it's a bug; think it's a config issue. Try adding this line at the top of main.ts:

/// <reference types="node" />

(Might pay to check the compiler options too.)
```