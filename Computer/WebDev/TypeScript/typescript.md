---
title: TypeScript
---

# Type Alias

Use keyword `type`. Name convention is **PascalCase**.

## Union type

Uses type _union operator_ `|` to make less specific type.

```typescript
type Progress = "Not started" | "In progress" | "Done";
type Address = string | string[];
```

## Intersect object type

Use type _intersect operator_ `&` to make a more specific object type. It works
like class inheritance. In fact it function the same as `interface extends`.

```typescript
type ClubMember = {
  name: string;
  id: string;
};

type Role = "Chairperson" | "General secretary" | "Financial secretary";

type ClubExco = ClubMember & {
  role: Role;
};
```

# Interface

# Describing Types

## Primitive types

Unlike in JSDoc, no need to capitalise the first letter of the primitive type in
TypeScript. So it is `number` not `Number`.

## Functions

Use arrow function notation:

- `() => void`
- `(arg: string) => number`

## Objects

### Direct description

```typescript
type ButtonProps = {
  type: "submit" | "reset" | "button"; // union typed field
  autoFocus?: boolean; // optional field
};
```

### Arbitrary fields

```typescript
type ObjectWithUnknownFields = {
  [key: string]: any;
};
```

```typescript
type ObjectUsingRecord = Record<string, any>;
```

### Omission

Use `Omit<T, K>`

```typescript
type User = {
  sessionId: string;
  name: string;
};

type Guest = Omit<User, "name">; // takes the User type and omit "name" property
```

## Generating types from types

- `keyof MyType` or `keyof typeof MyObj`

## Unknown types

`unknown` type is especially useful for describing data coming from external
source. You should use `unknown` over `any`.

```typescript
const data: unkonwn = fetch("https://example.com/api/givemecatcans");
```

In this case, you would still get error when invoking, say,
`data.name.toUpperCase()`, because `data` is of type `unknown`. So you may want
to use some _schema_ validators (e.g. Zod) to parse the data, before operating
on them.

Another useful trick is to use the library `ts-reset` when fetching data. It
helps make the default type for fetched data to be `unknown` and also some other
benefits.

# Function Argument and Return Type

## Argument and return type

```typescript
function hello(name: string): string {
  return `Hello ${name}!`;
}
```

## Inferred argument type

You can do inferred typing by giving a **default** value.

```typescript
function hello(name = "stranger"): string {
  return `Hello ${name}!`;
}
```

## Optional argument

Add `?` after the parameter identifier.

```typescript
function hello(firstName: string, lastName?: string): string {
  return `Hello ${firstName}` + lastName ?? ` ${lastName}` + "!";
}
```

# Function Overloading

You can define multiple function signatures for the same function name. Note
that you can still define the implementation (i.e. body) once.

# Type Assertion

## Asserting type

Use `as` keyword.

```typescript
const previousName = localStorage.getItem("playerName") as string;
```

## As const

For array and object, asserting them `as const` make them `readonly`, i.e. the
content is **immutable**. Using this can help further type inferring,
intersection or union. See the following example.

```typescript
const PrimaryColors = ["Red", "Yellow", "Blue"] as const;
export default function PrimaryColorBoxes() {
  return (
    <>
      {PrimaryColors.map((color) => {
        // `color` has inferred type = "Red" | "Yellow" | "Blue" because we
        // asserted `PrimaryColors` as const.
        // Otherwise `PrimaryColors` would have an inferred type `string[]` and
        // `color` here has inferred type `string`.
      })}
    </>
  );
}
```

## Assert non-nullable

Append `!` to the value to assert that it is non-nullable, meaning it is not
null or undefined. This is useful when you already filter out the possibilities
of nullable values, e.g. with `flatMap()`.

## Assert boolean

By default, you cannot use, say a number, in place of a boolean. To be able to
do this, you need to prepend `!!` in front to use it as boolean. Practically
it's just applying the `!` operator twice.

The other way is use `Boolean(value)`.

# Generics

## Intro to Generics

Generics is used to describe the **relationship** of types without restricting
what types they are. The convention is to use `T`, then `K` as the identifier
for a generic type. But any uppercase letter is good if it helps to make it
clear what kinds of types they might be.

Generics are like interface, and you can use the keyword `extends` to make it
more specific. See
[Objects with arbitrary fields](#objects-with-arbitrary-fields) for an example.

## Functions

```typescript
function wrapInArray<T>(value: T): T[] {
  // <T> is to tell TypeScript you are using T to represent a generic type
  return [value];
}

const wrapInArray2 = <T>(value: T): T[] => [value];
// The comma after T is necessary for arrow functions, especially in tsx file,
// to distinguish it from JSX.
```

## Objects

```typescript
type ValueControllerProps<T> = {
  value: T;
  valueHistory: T[];
};

export default function ValueController<T>({
  value,
  valueHistory,
}: ValueIndicatorProps<T>) {
  // ...
}

export default function ValueControllerV2<T>({ value: T, valueHistory: T[] }) {
  // ...
}
```

# Using Type Predicates for Type Guards

```ts
function isString(test: any): test is string {
  return typeof test === "string";
}

function example(foo: any) {
  if (isString(foo)) {
    console.log("it is a string" + foo);
    console.log(foo.length); // string function
  }
}
example("hello world");
```

Using the **_type predicate_** test is string in the above format (instead of
just using boolean for the return type), after `isString()` is called, if the
function returns true, TypeScript will narrow the type to string in any block
guarded by a call to the function. The compiler will think that foo is string in
the below-guarded block (and ONLY in the below-guarded block)

# Export types

Can define the types in the file `/lib/types.ts` and export normally:

```typescript
export type PrimaryColors = "red" | "blue" | "yellow";
```

```typescript
import { type PrimaryColors } from "@/lib/type.ts";
// the keyword `type` is not necessary, it's just to make it explicit
```

## Declaration files

You may see types definition in files like `types.d.ts`. For you own custom
types, don't use `.d.ts` extensions. For they are for types **_declaration
files_** that declares the types of exports from third-party libraries.

# Third-Party Library Types

Files with type definitions are in `/node_modules/@types/`.

A good library that provides TypeScript type definitions for old popular
third-party libraries is
[DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped.)

# Tsconfig

Next.js 2023 default:

```json
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [
      {
        "name": "next"
      }
    ],
    "paths": {
      "@/*": ["./*"]
    }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx", ".next/types/**/*.ts"],
  "exclude": ["node_modules"]
}
```

## With JavaScript

The presence of `tsconfig.json` nullifies `jsconfig.json`. So if you don't
include JavaScript files by adding `**/*.js` and `**/*.jsx` at the **very
front**, the settings in either config file won't applies to JS files. For
example, path resolution won't be done in a JS file, and intellisence would fail
when you do import aliases with `@`.

# Problems and tricks

## Handling unknown readonly field that might have different shape

In the following code, if we don't type cast `keySpec` in the function
`as unknown as KeySpec`, there will be a compilation error at the header of the
for-loop looping `reseveredKeys`, because `reservedKeys` may not exist as a
field inside an object of a readonly field.

```ts
const scales = {
  generalDepression: {
    keys: [1, 2, 5, 6, 8, 9, 11, 13, 21, 26, 30, 31, 40, 48, 51, 52, 57, 61],
    reversedKeys: [27, 64],
  },
  dysphoria: {
    keys: [2, 5, 8, 9, 21, 31, 40, 48, 57, 61],
  },
  lassitude: {
    keys: [6, 29, 30, 43, 54, 55],
  },
} as const;

type Subscale = keyof typeof scales;

type KeySpec = {
  keys?: number[];
  reversedKeys?: number[];
};

export function idas2Score(subscale: Subscale, answers: number[]): number {
  if (scales[subscale] == null)
    throw new Error(`IDAS-II doesn't have subscale "${subscale}".`);

  const keySpec = scales[subscale] as unknown as KeySpec;
  let score = 0;

  for (const key of keySpec?.keys ?? []) {
    score += answers[key - 1] + 1; // +1 to adjust for 0-based stored score
  }

  for (const key of keySpec?.reversedKeys ?? []) {
    score += 4 - answers[key - 1] + 1; // +1 to adjust for 0-based stored score
  }

  return score;
}
```

## Objects with arbitrary fields

```ts
/**
 * @returns the min and max value for a field in a dataset
 */
export function range<O, K extends keyof O>(
  data: O[],
  field: K,
): [number, number] {
  return data.reduce(
    (prev, curr) => {
      if (+curr[field] < prev[0]) prev[0] = +curr[field];
      if (+curr[field] > prev[1]) prev[1] = +curr[field];
      return prev;
    },
    [+data[0][field], +data[0][field]],
  );
}
```

# Intelliscense

You need to in development mode, and you need to use the browser to run the page
once to trigger the compiling first, before you can have intelliscense.

# Using with JavaScript

## Setting up tsconfig

In `tsconfig.js`, the most relevant `compilerOptions` are:

- `allowJs`: Necessary. Allows you to use JavaScript files along side TypeScript
  files.
- `outDir`: Necessary unless enable `noEmit` flag. Since all files will be
  transpiled, it is necessary to output the resulting JavaScript into a
  different directory otherwise the input .js files would be overwritten.
- `checkJs`: whether also to do type checking in JavaScript files. If you enable
  this, you should declare types and import types with JSDoc. Optional.

See
[Mixing JavaScript and TypeScript in Node.js](https://stackoverflow.com/questions/49640121/mixing-javascript-and-typescript-in-node-js).

## Handling types in JavaScript files

```js
/** @typedef {import("./types").NewsItem} NewsItem */
```

- `@typedef` tells the compiler you are defining a type.
- Within curly braces is the actual type being defined.
  - `import()` is just like regular ES module `import`. Here it's importing from
    a local file `types`, probably [`types.d.ts`](#declaration-files) or
    `types.ts`.
  - Then you are importing the named export type `NewsItem` from that file
- Lastly you name the newly defined type as `NewsItem`.

To type cast:

```js
const res = fetch(url);
const feed = /**@type NewsFeed */ res.json();
```

# React

## React types

When you don't know what type a react token is, you can hover same kind of token
existing and see what type it is.

- `React.CSSProperties`
- `JSX.Element`: most of the time you let TypeScript do inferred type
- `React.ReactNode`: useful for `children` prop. Is union of:
  - JSX.Element
  - string
  - boolean
- `React.ComponentProps<"button">`: all valid props for `<button>`. But better
  to use:
  - `React.ComponentPropsWithRef<"button">` for components that use `ref`; and
  - `React.ComponentPropsWithoutRef<"button">` for components that doesn't
  - Usually useful to use with type intersect operator `&`

## Hooks

### useState

#### Define state type

Usually TypeScript infers the type when you give an initial value. You can
define the type explicitly:

```typescript
const [count, setCount] = useState<number>(0);
const [user, setUser] = useState<User | null>(null); // use a custom User type
```

#### setState dispatcher

You don't need to memorize the set state function type. Hover to see the types,
e.g.:

- `React.Dispatch<React.SetStateAction<number>>`: set state dispatcher for a
  number typed state

### useRef

```typescript
const ref = useRef<HTMLButtonElement | null>(null);
return <button ref={ref}>Click me!</button>;
```

## Event handlers

When you define a event handler in the right context, e.g. inside curly braces
of `onClick={}` in a component, then the type of the first argument `e` is
automatically inferred.

If you need to define the function outside, you can use hover and copy the type
shown and then paste it.

## Common practical examples

### Props type definition with CSS

```typescript
type ButtonProps = {
  style: React.CSSProperties;
};

export default function Button({ style }: ButtonProps) {}
```

### Inline type declaration

```typescript
export default function Button({
  fontSize,
  count,
}: {
  fontSize: number;
  count: number;
}) {}
```

### Children prop

```typescript
export default function Button({ children }: React.ReactNode) {}
```

# Next

The file `next-env.d.ts` in the root is to provide Next.js own's typing. Like
the `next` property for `fetch()` `requestInit` argument.

# Bugs and Incompatibility

Note that functional components would lost its generics (it got changed into
`unknown`) type definition, if you do a `dynamic()` export.

```ts
export default function ThisWorks<T>({
  data,
  numKey,
}: {
  data: T[];
  numKey: NumKeys<T>;
}) {}
```

The above export works, but the following dynamic export would change the type
of the arguments. In this case, `data` becomes `unknown[]` and `numKey` becomes
`never`. Same for using `dynamic()` to import.

```ts
export default dynamic(() => Promise.resolve(CategoricalBarChart), {
  ssr: false,
});
```

# 🧭 Navigation

- [🔼 Back to top](#)
- [📑 Notes Index](../../index.md)
- [🗃️ Index](/media/mikeX/Nextcloud/index.md)
