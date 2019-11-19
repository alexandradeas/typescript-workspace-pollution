## Overview
A demo TS project to show how one yarn workspace depending on another workspace
with weaker compiler options fails type checking.

workspace-a is a library with no additional type configuration enabled (the most
important being that 'noImplicitAny' is not enabled). This library exposes one
function `log`. `log` does not provide any type annotations and therefore its
type is inferred as `const log: (msg: any) => void`.

workspace-b is an application with the 'strict' compiler option enabled. This
means that the 'noImplicitAny' rule is enabled. workspace-b has a depenency on
workspace-a. workspace-b has 'node_modules' excluded as part of the
`tsconfig.json`

## The Issue

When building workspace-b TypeScript returns an error for `workspace-a#log`
because its parameter is implicitly typed as any.

```
../workspace-a/src/index.ts:1:21 - error TS7006: Parameter 'msg' implicitly has an 'any' type.

1 export const log = (msg) => {
                      ~~~


Found 1 error.

error Command failed with exit code 2.
```

## Steps to reproduce
```sh
yarn install
cd workspace-b
yarn tsc
```
