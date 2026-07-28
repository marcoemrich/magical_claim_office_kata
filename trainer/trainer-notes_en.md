# Trainer notes for the "Magical Claim Office" kata

Accompanying material for the trainer. **Not for participants.**

Contains the claims office's rulings on every ambiguity — that is, the
answers the group is meant to work out for themselves.

> *Deutsche Fassung: [`trainer-notes_de.md`](trainer-notes_de.md)*

A suggested workshop schedule lives separately in
[`workshop-durchfuehrung_en.md`](workshop-durchfuehrung_en.md).

## Setting

Task: [`kata_en.md`](../kata_en.md).

Input and output format for participants:
[`io-format_en.md`](../io-format_en.md). It describes the interface
only, no calculation rules — the ambiguities therefore remain
untouched.

Verification scenarios (for checking values / demonstration once the
group has made its own rulings): [`verifikation/`](verifikation/) — ten
scenarios explained in
[`verifikation/instructions_en.md`](verifikation/instructions_en.md).

## List of ambiguities

Five main ambiguities plus one sub-ambiguity. The lettering follows the
kata's internal numbering.

### A — Scoring a set with extras

**Question:** How is a collection of 4 or more alike components scored?

**Ruling:** A building block counts **only at exactly three**
components. At 4 or more, everything is counted individually — no
"greedy" block forming, no "one block, remainder individually".

**Examples to pin down during mapping:**
- 3 runes → 60 G base premium (block)
- 4 runes → 100 G base premium (4 × 25, no block)
- 7 runes → 175 G base premium (7 × 25, no block)

**Trainer's note:** This reading is the *most expensive* of the three
plausible ones — fitting for the stingy tone. Participants typically
lean towards a greedy max-block. If nobody arrives at "strictly three
only", the trainer can deploy the assessor's character ("Three *alike*
components constitute a block. Three.").

### Aₐ — "alike" as a term

**Question:** What does "alike" mean? Same type, same category, same
material?

**Ruling:** Same **type designator**. Runes and moonstones are never
alike to one another, even though both are "components".

**Examples:**
- 2 runes + 1 moonstone → 75 G (no block, different types)
- 3 runes + 3 moonstones → 120 G (two separate blocks)

**Trainer's note:** A classic terminological stumble. During mapping,
somebody typically asks "hold on, what does alike mean?". This is
*constructively hidden information* — the question has to be asked
actively, it is not answered in the rules.

### B₂ — Deductible per damage event

**Question:** Does the 100 G deductible apply once per claim (e.g. one
dragon attack) or per damaged item?

**Ruling:** The deductible is deducted **per item**. A dragon attack
damaging two items costs two deductibles of 100 G.

**Examples:**
- Dragon attack: sword (500 G) + amulet (300 G) →
  (500 − 100) + (300 − 100) = 600 G payout

**Trainer's note:** Expect discussion in the workshop. The ruling fits
the stingy tone — the office happily retains more.

### C — "first insurance" as a term

**Question:** Does the 10 % initial assessment surcharge refer to *the
customer's first contract* (customer-related) or to *the first policy
for an item* (item-related)?

**Ruling:** **Item-related.** Even a regular customer insuring a new
item pays the initial surcharge — because the item is under assessment
for the first time.

**Examples:**
- Regular customer of 3 years, second contract with a new sword → +10 %
  (first insurance of the sword) AND −15 % (customer's follow-up
  contract), both at once.

**Trainer's note:** The two clauses "first insurance +10 %" and "from
the second contract −15 %" appear contradictory at first. The office's
reading is that they apply *in parallel* — item-first plus
customer-follow-up.

**How the JSON reads:** Items in the scenario JSON have no identity (no
`id`, no "wasInsuredBefore" flag). Consequence: every item implicitly
counts as a first insurance — the +10 % surcharge therefore applies at
*every* quote step to *every* item. The −15 % follow-up discount, by
contrast, is readable from the JSON: it applies at every `quote` step
*after the first* in the `steps` array (same customer throughout the
scenario). Frequent participant question: "How can I see that the sword
was insured before?" — Answer: you can't, and that is deliberate.

### D — Order of factors for modifiers

**Question:** How are the modifiers (curse, enchantment, loyalty, etc.)
applied to the base price? Additively? Multiplicatively? In what order?

**Ruling:** **Additively on the base price.** All percentages are
converted into absolute gold pieces (each as a percentage of the base
price) and added to or subtracted from the policy base price.

**Extension — item vs. policy scope:** Item-specific modifiers (curse,
high enchantment) act on the **item base price** of the affected item.
Policy modifiers (loyalty discount, first insurance, follow-up
contract) act on the **policy base price** (the sum of all item base
premiums).

**Examples:**
- Cursed level-7 sword, 3-year regular customer, second contract, base
  price 100 G:
  100 + 50 (curse) + 30 (high enchantment) − 20 (loyalty)
  + 10 (first ins.) − 15 (follow-up) = 155 G
  + 5 G processing fee = **160 G**
- Policy with a cursed sword (100) + amulet (60), newcomer:
  policy base price 160 G; curse surcharge +50 G (50 % of the sword
  base price); initial surcharge +16 G (10 % of the policy base price);
  subtotal 226 G + 5 = **231 G**

**Trainer's note:** Additive is the *everyday reading* (e-commerce
discount codes). If the mapping chooses multiplicative, *that* is a
valid workshop experience too ("your tariff calculates differently from
the office's tariff" — a valuable insight about pinning down
requirements).

### F' — Risk threshold vs. dragon material

**Question:** What happens with an item that triggers *both* claim
settlement clauses — dragon material AND enchantment level ≥ 8?

**Ruling:** The **50 % clause wins.** Risk threshold beats material
clause.

**Order relative to the deductible:** **Apply the reimbursement clause
first, then subtract the deductible.**

**Examples:**
- Dragon sword, enchantment 9, damage 1000 G →
  500 G (50 %) − 100 G deductible = **400 G payout**
- Dragon sword, enchantment 5, damage 800 G →
  800 G (full, dragon material) − 100 G deductible = **700 G payout**
- Steel sword, enchantment 9, damage 1000 G →
  500 G (50 %) − 100 G deductible = **400 G payout**

**Trainer's note:** The order between clause and deductible is a hidden
ambiguity within this question. If nobody raises it, the trainer can
ask using a concrete example ("dragon material sword, ench 9, damage
1000 G — what does the office pay?").

## Further important rulings

These points are not ambiguities in the narrow sense, but they come up
frequently.

### Processing fee

- Always 5 G on top, *after* all modifiers, *never* discounted.
- Also for an empty item list: premium 0 G + 5 G fee = 5 G.

### Rounding

- All amounts in the office's favour: **round premiums up, round
  payouts down**.
- Round **at the end** of the calculation; intermediate values stay
  fractional.

### No cap on the payout

- There is **no upper limit** on the total payout. Every claim is
  calculated independently; a policy may pay out arbitrarily much over
  time.
- Consequence: the policy needs **no cumulative state**. Anyone
  carrying a "consumed amount" anyway has made work for themselves that
  nobody here asked for.
- The insured value (1000 G for a sword, etc.) does appear in the price
  list but is never needed. **Frequent participant question:** "What is
  the insured value for, then?" Answer, in character: "For the ledger.
  The assessor enters it. What the office later does with it is not
  your concern."

### Several items of the same type

- A policy may contain several items of the same type (two swords,
  three potions).
- One deductible per `damages` entry — even when both entries have the
  same `itemType`.
- If the number of damage entries exceeds the number of items in the
  policy → **reject the claim entirely** (bureaucratically strict).

### Unknown items

- Item with an unknown type (not in the main item table, not a
  component type) → error / complete rejection.
- Damage entry whose item is not in the policy → error.
- Negative damage amount → error.

### Edge cases

- Empty item list in `quote` → premium 0 G (= 5 G with the fee), no
  error.
