# Input and output format

This description complements the task in [`kata_en.md`](kata_en.md). It
says *how* the program is invoked — not *what* it computes. The
calculation rules live exclusively in the task.

> *Deutsche Fassung: [`io-format.md`](io-format.md)*

The field names are **binding** — renaming or restructuring them is not
at your discretion.

## The program at a glance

What you build is a command-line program. It reads a JSON document from
standard input and writes a JSON document to standard output. No files,
no database, no interactive prompts — in one end, out the other.

```
echo '{ ... }' | claim-office
{ ... }
```

One invocation processes one **scenario**: a customer and a sequence of
operations. After the invocation the program is done; it remembers
nothing beyond it.

## The input

The input document has two fields: `customer` and `steps`.

```json
{
  "customer": { "yearsWithMHPCO": 0 },
  "steps": [ ... ]
}
```

### `customer` — the customer

There is **exactly one** customer for the whole scenario. They have one
field:

- `yearsWithMHPCO` — how many full years the customer has been a
  customer, as an integer. This determines whether the loyalty discount
  applies.

### `steps` — the operations

`steps` is a list of operations that are processed **in the order
given**. Every operation has an `op` field stating its kind: `"quote"`
or `"claim"`.

The order matters: a later operation may refer to a policy created by
an earlier one. Whether a contract is a follow-up contract likewise
depends on what happened earlier in the same scenario.

#### Operation `quote` — calculate a premium

```json
{
  "op": "quote",
  "items": [
    { "type": "sword", "material": "steel", "enchantment": 3, "cursed": false }
  ]
}
```

- `items` — the list of items to be insured.

Every item has:

- `type` — the kind of item. Provided for are `sword`, `amulet`,
  `staff`, `potion` (main items) as well as `rune` and `moonstone`
  (components).
- `material` — the material, for example `steel`, `silver`, `glass` or
  `dragon`. Only one value matters for the calculation; which one is
  stated in the task.
- `enchantment` — the enchantment level as an integer.
- `cursed` — whether the item is cursed, `true` or `false`.

For components, `material`, `enchantment` and `cursed` may be absent.
An item has **no identifier** — two swords in one policy are two
entries in the list and otherwise indistinguishable.

#### Operation `claim` — settle a claim

```json
{
  "op": "claim",
  "policy": 0,
  "incident": {
    "cause": "dragon attack",
    "damages": [
      { "itemType": "sword", "amount": 500 }
    ]
  }
}
```

- `policy` — which policy the damage refers to, as a **zero-based
  position in the `steps` array**. `0` therefore means the first
  operation of the scenario, and that one must be a `quote`.
- `incident` — the damage event:
  - `cause` — the cause as free text (`"dragon attack"`, `"fire"`,
    `"fall"`, …). It goes in the ledger and does **not** enter the
    calculation.
  - `damages` — the list of damaged items. Every entry has `itemType`
    (the kind of the damaged item, matching a `type` in the policy) and
    `amount` (the damage amount in gold pieces, an integer).

## The output

The output document has one field, `results`.

```json
{ "results": [ ... ] }
```

`results` is a list that is **exactly as long as `steps`** and in
**the same order**. The operation at position 0 corresponds to the
result at position 0.

Depending on the kind of operation, a result looks different:

- A `quote` yields `{ "premium": <integer> }` — the premium in gold
  pieces.
- A `claim` yields `{ "payout": <integer> }` — the payout in gold
  pieces.

All amounts are **integers**. How rounding works is stated in the task.

## A complete example

A customer, three years in business with the office, insures an amulet
and later reports fire damage.

**Input:**

```json
{
  "customer": { "yearsWithMHPCO": 3 },
  "steps": [
    {
      "op": "quote",
      "items": [
        { "type": "amulet", "material": "silver", "enchantment": 2, "cursed": false }
      ]
    },
    {
      "op": "claim",
      "policy": 0,
      "incident": {
        "cause": "fire",
        "damages": [
          { "itemType": "amulet", "amount": 200 }
        ]
      }
    }
  ]
}
```

**Output — the shape:**

```json
{
  "results": [
    { "premium": 59 },
    { "payout": 100 }
  ]
}
```

Both numbers follow from the rules in the task.

## What the format does not govern

This description fixes the shape of the documents, not their meaning.
What happens when an `items` list is empty, an unknown kind appears or
an `amount` is negative is not stated here.

The data model itself also leaves questions open: an item has no
identifier, and a `damages` entry names only a kind, not a particular
item.

Anyone with such questions should take them to the claims office.

## Technical requirements

- The program reads from standard input and writes to standard output.
- The output is a single JSON document.
- Programming language, internal structure and tooling are free, unless
  the team has agreed otherwise.
