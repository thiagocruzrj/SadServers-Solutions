# Runbook: Solve the Command Line Murders

**Prerequisites:** Read access to `/home/admin/clmystery/mystery/`.

---

## Step 1 — Extract the crime scene clues

```bash
grep "CLUE" /home/admin/clmystery/mystery/crimescene
```

Three CLUEs:
- Perpetrator is a **tall male, at least 6'**
- Has membership cards for the **Rotary Club** and the **Museum of Bash History**
- A woman named **Annabel** is a witness

---

## Step 2 — Find the witness Annabel

```bash
grep "^Annabel" /home/admin/clmystery/mystery/people
```

Note each Annabel's street and line number.

---

## Step 3 — Get the interview number

```bash
sed -n '179p' /home/admin/clmystery/mystery/streets/Buckingham_Place
```

Returns: `SEE INTERVIEW #699607`

---

## Step 4 — Read the witness interview

```bash
grep -i "car\|vehicle\|plate\|honda" /home/admin/clmystery/mystery/interviews/interview-699607
```

Clue: **blue Honda**, plate starts with `L337`, ends with `9`.

---

## Step 5 — Find matching vehicles

```bash
awk 'BEGIN{RS=""; FS="\n"} /Make: Honda/ && /Color: Blue/ && /License Plate L337.*9/' \
  /home/admin/clmystery/mystery/vehicles \
  | grep "Owner:"
```

Six candidates: Erika Owens, Aron Pilhofer, Heather Billings, Joe Germuska, Jeremy Bowers, Jacqui Maher.

---

## Step 6 — Filter by Museum of Bash History membership

```bash
grep -E "Erika Owens|Aron Pilhofer|Heather Billings|Joe Germuska|Jeremy Bowers|Jacqui Maher" \
  /home/admin/clmystery/mystery/memberships/Museum_of_Bash_History
```

Four remain: Aron Pilhofer, Joe Germuska, Jeremy Bowers, Jacqui Maher.

---

## Step 7 — Filter by Rotary Club membership

```bash
grep -E "Aron Pilhofer|Joe Germuska|Jeremy Bowers|Jacqui Maher" \
  /home/admin/clmystery/mystery/memberships/Rotary_Club
```

Only **Joe Germuska** is in both clubs.

---

## Step 8 — Write the solution

```bash
echo "Joe Germuska" > ~/mysolution
```

---

## Step 9 — Verify

```bash
md5sum ~/mysolution
```

Expected: `9bba101c7369f49ca890ea96aa242dd5`
