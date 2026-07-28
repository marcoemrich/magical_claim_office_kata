# Trainer material — Magical Claim Office

**Not for participants.** Everything in this directory gives away the
resolutions: which reading the claims office chose for each ambiguity,
and what values follow from it.

## Contents

| File | Contents |
|------|----------|
| [`trainer-notes_de.md`](trainer-notes_de.md) | Resolution of all ambiguities and the remaining rulings — German. |
| [`trainer-notes_en.md`](trainer-notes_en.md) | The same — English. |
| [`workshop-durchfuehrung_de.md`](workshop-durchfuehrung_de.md) | Suggested schedule, tips, stumbling blocks, tone — German. |
| [`workshop-durchfuehrung_en.md`](workshop-durchfuehrung_en.md) | The same — English. |
| [`verifikation/`](verifikation/) | Ten acceptance scenarios as JSON pairs, with instructions in [German](verifikation/anleitung_de.md) and [English](verifikation/instructions_en.md). |

The participant material is one level up: [`kata_de.md`](../kata_de.md)
and [`io-format_de.md`](../io-format_de.md), each also in English.

## Why a separate branch

This directory exists **only in the `trainer` branch**. The `main`
branch contains the participant material alone — so anyone cloning the
repo or downloading it as a ZIP does not get the resolutions by
accident.

As a trainer you work on `trainer`:

```bash
git clone git@github.com:marcoemrich/magical_claim_office_kata.git
cd magical_claim_office_kata
git checkout trainer
```

The group stays on `main`, where `trainer/` simply does not exist.

**When maintaining:** changes to the task belong on `main` and are
merged from there into `trainer`, not the other way round:

```bash
git checkout trainer && git merge main
```

That way `trainer` always stays ahead of `main` and the participant
files never drift apart.
