# Verification scenarios

Ten scenarios for checking an implementation against the claims
office's rulings. They are the acceptance tests of the kata.

**For the trainer**, not to be handed out to participants at the start.
The expected values give away every resolution of every ambiguity —
precisely what the group is meant to work out for itself. Handing them
out only makes sense once the group has made its own rulings.

> *Deutsche Fassung: [`anleitung_de.md`](anleitung_de.md)*

## Structure

Every scenario consists of two files:

- `<name>.input.json` — fed to the program on standard input
- `<name>.expected.json` — the expected document on standard output

The comparison is a **structural JSON comparison**, not a text
comparison. Indentation, key order and line breaks are irrelevant.

## Running them

By hand:

```bash
echo "$(cat szenarien/01-block-exact-three.input.json)" | claim-office
```

Then hold the result against `01-block-exact-three.expected.json`.

All scenarios at once, if `jq` is available:

```bash
for i in szenarien/*.input.json; do
  name=$(basename "$i" .input.json)
  expected="szenarien/$name.expected.json"
  actual=$(claim-office < "$i")
  if [ "$(echo "$actual" | jq -S .)" = "$(jq -S . "$expected")" ]; then
    echo "OK    $name"
  else
    echo "FAIL  $name"
    diff <(echo "$actual" | jq -S .) <(jq -S . "$expected")
  fi
done
```

`claim-office` stands for the invocation of your own implementation —
a different command depending on language and layout.

## The scenarios in detail

Scenarios 01–07 each check one ambiguity in isolation, 08–10 their
interplay. The "checks" column names the ruling the scenario pins down;
the derivations are in
[`../trainer-notes_en.md`](../trainer-notes_en.md).

| # | Scenario | checks | expected |
|---|----------|--------|----------|
| 01 | `block-exact-three` | building block at exactly three components | `premium: 71` |
| 02 | `block-not-four` | no block at four — everything individually | `premium: 115` |
| 03 | `alike-different-types` | "alike" means same type | `premium: 88` |
| 04 | `deductible-per-item` | deductible per item, not per event | `premium: 181`, `payout: 600` |
| 05 | `high-enchantment-clause` | 50 % clause from enchantment level 8 | `premium: 145`, `payout: 400` |
| 06 | `dragon-material-clause` | dragon material reimbursed in full | `premium: 115`, `payout: 700` |
| 07 | `clause-conflict` | with both clauses, the 50 % clause wins | `premium: 145`, `payout: 400` |
| 08 | `newcomer-cursed` | curse surcharge for a new customer | `premium: 165` |
| 09 | `follow-up-customer` | follow-up contract and loyalty discount together | `premium: 41`, then `160` |
| 10 | `multi-items-same-type` | two alike items in one policy | `premium: 225`, `payout: 2100` |

### Two derivations to check by hand

**02 — four runes, new customer.** Four runes form no block, so
4 × 25 = 100 G base premium. On top of that the initial assessment
surcharge of 10 % = 10 G. That makes 110 G, plus the 5 G processing
fee: **115 G**. The comparison with scenario 01 (three runes, 71 G) is
the point being made — the fourth item makes the policy markedly more
expensive.

**07 — dragon sword, enchantment level 9, damage 1000 G.** Both clauses
apply; the 50 % clause beats the material clause. So 1000 × 0.5 =
500 G, less the 100 G deductible: **400 G**. That is the same payout as
in the steel scenario 05 — which is exactly what this scenario
demonstrates.

## When the group has decided differently

If the team arrives at a different, internally coherent reading, then
scenarios here will fail without the implementation being wrong. That
is not a defect but the instructive part: the scenarios are the ruling
of *one* branch of the claims office, not the only conceivable truth.

Recommendation in that case: name the affected scenarios, write the
differing ruling alongside, and leave both standing. The trainer can
then introduce the suite as "the branch office in the capital
calculates like this".
