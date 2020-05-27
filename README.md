<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Installation](#installation)
- [Usage](#usage)
  - [Define a sharp](#define-a-sharp)
    - [Primitive](#primitive)
    - [Combinators](#combinators)
      - [The `s.Literal` constructor](#the-sliteral-constructor)
      - [The `s.Nullable` constructor](#the-snullable-constructor)
      - [The `s.Optional` constructor](#the-soptional-constructor)
      - [The `s.Struct` constructor](#the-sstruct-constructor)
      - [The `s.Partial` constructor](#the-spartial-constructor)
      - [The `s.Map` constructor](#the-smap-constructor)
      - [The `s.Array` constructor](#the-sarray-constructor)
      - [The `s.Tuple` constructor](#the-stuple-constructor)
      - [The `s.Intersection` constructor](#the-sintersection-constructor)
      - [The `s.Union` constructor](#the-sunion-constructor)
    - [Brand](#brand)
  - [Assertion](#assertion)
- [Roadmap](#roadmap)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Installation

`npm i toa-sharp`

## Usage

### Define a sharp

```ts
const sharp = Sharp.make((s) =>
  s.Struct({
    id: s.ObjectID,
    name: s.String,
    age: s.Optional(s.Number)
    address: s.Array(s.String)
  })
)

// stands for type => {
//   id: ObjectID // string with objectId constraint
//   name: string
//   age: number | undefined
//   address: string[]
// }
```

#### Primitive

- `s.Never`
- `s.String`
- `s.Number`
- `s.Boolean`
- `s.Unknown`

#### Combinators

##### The `s.Literal` constructor

- `s.Literal(null)` => `null`
- `s.Literal(undefined)` => `undefined`
- `s.Literal('ON')` => `'ON'`
- `s.Literal('ON', 'OFF')` => `'ON' | 'OFF'`

##### The `s.Nullable` constructor

`s.Nullable(s.String)` => `string | null`

##### The `s.Optional` constructor

`s.Optional(s.String)` => `string | undefined`

Notice that it DOES NOT mean the key is optional (`key?: string`)

##### The `s.Struct` constructor

`s.Struct({ id: s.String, time: s.Number })` => `{ id: string, time: number }`

##### The `s.Partial` constructor

`s.Partial({ id: s.String, time: s.Number })` => `{ id?: string, time?: number }`

##### The `s.Map` constructor

`s.Map(s.Boolean)` => `Record<string, boolean>`

##### The `s.Array` constructor

`s.Array(s.Number)` => `number[]`

##### The `s.Tuple` constructor

`s.Tuple(s.String, s.Number)` => `[string, number]`

##### The `s.Intersection` constructor

`s.Intersection(s.Struct({ name: s.String }), s.Partial({ age: s.Number }))` => `{ name: string } & { age?: number }`

##### The `s.Union` constructor

`s.Union(s.String, s.Number)` => `string | number`

#### Brand

- the `s.ObjectID` is string with ObjectID constraint, it can pass to string

### Assertion

Say we are using koa(with body-parser), to assert body sharp you can

```ts
import { Sharp, assert } from "toa-sharp";
import { Errors } from "./errors";

const Body = Sharp.make((s) => S.Struct({ id: s.ObjectID, text: s.String }));

async function handler(ctx: Context) {
  assert(ctx.request.body, Body, new Errors.InvalidParams());
  const { id, text } = ctx.request.body; // with type { id: ObjectID, text: string }
}
```

The third argument can receive a `Error | ErrorContructor`, the Error instance will be throw directly while the ErrorContructor will receive error message as first argument.
By default it's `TypeError` constructor.

## Roadmap

- Add branded type such as Int(number), Positive(number), Email(string), Url(string), etc.
