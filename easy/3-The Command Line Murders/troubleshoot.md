# Troubleshoot: Investigate the Command Line Murders

**Goal:** Follow the clues embedded in the mystery files to identify the murderer's name.

All clues live under `/home/admin/clmystery/mystery/`. Lines containing actual clues are marked with the word `CLUE` in all caps; everything else in those files is flavour text.

---

## Step 1 — Extract the clues from the crime scene

```bash
grep "CLUE" /home/admin/clmystery/mystery/crimescene
```

`grep` scans the file and prints only lines matching the pattern. The crime scene contains three CLUEs:

1. The perpetrator is a **tall male, at least 6'**.
2. He has membership cards for the **Rotary Club** and the **Museum of Bash History**.
3. A woman named **Annabel** (blond spiky hair, New Zealand accent) left right before the shots — she is a **witness**.

---

## Step 2 — Find Annabel's full name

```bash
grep "^Annabel" /home/admin/clmystery/mystery/people
```

`^` anchors the match to the start of the line, so only names beginning with "Annabel" are returned. The `people` file format is:

```
NAME    GENDER    AGE    ADDRESS
```

Several Annabels may appear. The address field tells you which street she lives on and which line number in that street file is hers.

---

## Step 3 — Get Annabel's interview number

For each Annabel, read her line from her street file:

```bash
sed -n '<LINE>p' /home/admin/clmystery/mystery/streets/<STREET_NAME>
```

- `-n` suppresses all output except what you explicitly print
- `'<LINE>p'` prints only the specified line number

If the line contains `SEE INTERVIEW #XXXXXXXX`, that Annabel is a witness. Note the interview number.

---

## Step 4 — Read Annabel's interview

```bash
grep -i "CLUE\|car\|vehicle\|plate\|honda" /home/admin/clmystery/mystery/interviews/interview-<NUMBER>
```

The relevant Annabel's interview (interview-699607, Annabel Church at Buckingham Place line 179) contains a vehicle description of the car that fled the scene:

- **Color:** Blue
- **Make:** Honda
- **License plate:** starts with `L337`, ends with `9`

---

## Step 5 — Find matching vehicles

```bash
awk 'BEGIN{RS=""; FS="\n"} /Make: Honda/ && /Color: Blue/ && /License Plate L337.*9/' \
  /home/admin/clmystery/mystery/vehicles \
  | grep "Owner:"
```

`awk` with `RS=""` treats each blank-line-delimited block as one record. All three conditions must be true in the same block before it prints. `grep "Owner:"` then extracts just the owner line from those matching blocks.

This produces six candidates.

---

## Step 6 — Filter by Museum of Bash History membership

```bash
grep -E "Erika Owens|Aron Pilhofer|Heather Billings|Joe Germuska|Jeremy Bowers|Jacqui Maher" \
  /home/admin/clmystery/mystery/memberships/Museum_of_Bash_History
```

Four of the six are members: Aron Pilhofer, Joe Germuska, Jeremy Bowers, Jacqui Maher.

- `-E` — extended regex, allowing the `|` alternation operator to match multiple names in one pass

---

## Step 7 — Filter by Rotary Club membership

This is the **SadServers twist** — the wallet also contained a Rotary Club card, which is absent in the original puzzle.

```bash
grep -E "Aron Pilhofer|Joe Germuska|Jeremy Bowers|Jacqui Maher" \
  /home/admin/clmystery/mystery/memberships/Rotary_Club
```

Only **Joe Germuska** appears in both clubs.

---

## Step 8 — Confirm gender and height

```bash
awk -F'\t' '$1 == "Joe Germuska" {print $1, $2}' /home/admin/clmystery/mystery/people
grep -A 4 "Owner: Joe Germuska" /home/admin/clmystery/mystery/vehicles | grep "Height:"
```

- Gender: **M**
- Height: **6'2"** ✓

Joe Germuska satisfies all five constraints:

| Constraint | Value |
|------------|-------|
| Museum of Bash History member | ✓ |
| Rotary Club member | ✓ |
| Male | ✓ |
| Height ≥ 6' | 6'2" |
| Blue Honda, plate L337...9 | L337DV9 |

---

## Key commands recap

| Command | What it does |
|---------|-------------|
| `grep "CLUE" FILE` | Print only lines containing the word CLUE |
| `grep "^Annabel" FILE` | Print lines starting with Annabel |
| `sed -n '<N>p' FILE` | Print only line N of a file |
| `grep -E "A\|B\|C" FILE` | Print lines matching any of the alternatives |
| `awk -F'\t' '$1 == "Name"' FILE` | Match a specific name in a tab-delimited file |
| `awk 'BEGIN{RS=""} /PATTERN/' FILE` | Match across multi-line records separated by blank lines |
