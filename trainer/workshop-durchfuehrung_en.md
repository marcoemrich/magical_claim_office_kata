# Running the workshop

One suggestion for how to use the kata in a workshop — one of several.
The substantive rulings the schedule refers to are in
[`trainer-notes_en.md`](trainer-notes_en.md).

> *Deutsche Fassung: [`workshop-durchfuehrung_de.md`](workshop-durchfuehrung_de.md)*

## Suggested schedule (half day, approx. 3 h)

1. **Read out / hand out the setting** ([`kata_en.md`](../kata_en.md)) — approx. 10 min
2. **Example Mapping in small groups** — approx. 45 min
   - Story card (yellow): "calculate a policy / settle a claim"
   - Rule cards (blue): extracted from the setting
   - Example cards (green): at least one example per rule
   - Question cards (red): open points that need the PO/trainer
3. **Plenary: answering questions** — trainer as PO, answers the red
   cards according to the rulings in the notes — approx. 20 min
4. **Implementation in pair or mob programming** — approx. 75 min
5. **Comparison with the verification scenarios** — trainer demonstrates
   [`verifikation/szenarien/`](verifikation/szenarien/) as acceptance
   tests — approx. 20 min

The [input and output format](../io-format_en.md) sensibly joins at
step 4 — or already at step 1, if the group wants to know early what it
is heading towards.

## Tips for the trainer

- **Only answer red cards once they are actually asked.** If the group
  does not discover an ambiguity itself, that is part of the
  experience — the "PO" points out gaps only during implementation.
- **Stay in character.** The trainer is not a trainer but *the assessor
  of the claims office*. Bureaucratic, mildly passive-aggressive, with
  references to "the house rules".
- **On conflicts between the group's ruling and the office's:** be
  flexible. If the group consistently chooses a different reading and
  stays coherent, that is pedagogically valuable. The verification
  scenarios can then be presented as "the branch office in the capital
  calculates like this".
- **Don't overdo the arithmetic.** Under time pressure it is enough for
  the rules to be implemented *qualitatively* correctly — the trainer
  can check the exact gold pieces afterwards.
- **Don't invent rules.** What is not in `kata_en.md` does not exist. An
  extra rule slipped in during the plenary makes the task unevenly
  harder for all groups.

## Common stumbling blocks

- **Insured value with no use:** the insured value is never needed.
  Participants often look for a rule for it. The answer is in the notes
  under "No cap on the payout".
- **Combining modifiers:** groups often waver between additive and
  multiplicative. The trainer can pin this down with a worked example.
- **The F' conflict:** it is often missed that both clauses can apply at
  once. The hint example "dragon sword at level 9" helps.
- **"alike":** participants sometimes read this as "same category" (all
  components alike). The trainer pins it to the exact type.

## The verification scenarios as training material

The ten scenarios in [`verifikation/szenarien/`](verifikation/szenarien/)
can be shown as **acceptance tests** once the group has made its own
rulings (invocation examples and expectations table:
[`verifikation/instructions_en.md`](verifikation/instructions_en.md)):

- 01–07: one test per ambiguity → for discussing individual rule
  resolutions
  - 01 block of exactly three, 02 no block at four, 03 "alike" by exact
    type, 04 deductible per item, 05 the 50 % clause, 06 dragon
    material, 07 clause conflict
- 08–10: combined tests → showing interactions
  - 08 newcomer with a cursed item, 09 follow-up contract, 10 several
    items of the same type

## Notes on tone

The claims office is conceived as a *bureaucratic, stingy insurer*.
When answering red cards:

- Rulings tend to favour the house (smaller sets, higher premiums,
  lower payouts).
- Linguistically neutral or faintly officious — no emotional
  justification, but "the house rules say", "it has been so since
  1612", "the assessor has decided".
- On detailed questions, do *not* immediately supply the full answer —
  first find out whether the group arrives at a reading itself. Only
  once they are stuck, answer with the ruling.
