# Carp-Manning direction lookup table

District-court analogue of Tables A.4 / A.5 in Sapiro-Gheiler & Kastellec (2026). Derived from
`data/raw/codebook-5-24-2016.pdf` pp. 23–38, with `category` and case counts verified against
`data/raw/carp_manning.dta` (n = 110,977).

**Read this first.** Every Carp-Manning case type states its rule in the same form:
`1 = Decision for the petitioner (liberal)` / `0 = Decision against the petitioner
(conservative)`. "Petitioner" here is a term of art meaning **the focal interest named in that
case type's codebook entry**, *not* whichever party filed the motion. In several case types the
focal interest is a government body, so a win for the government is coded liberal. The whole
purpose of this table is to make that mapping mechanical and remove it from the model's
judgment.

The rule is therefore uniform:

```
focal interest prevailed      → libcon = 1 (liberal)
focal interest did not prevail → libcon = 0 (conservative)
```

and all the substance lives in **who the focal interest is**.

---

## Table

`Cat` = `category` as recorded in the data (1 criminal justice, 2 civil liberties/rights,
3 economic regulation & labor). `N` = rows in the 2016 release.

| # | Case type | Cat | N | Focal interest — a decision **for** this party is liberal |
|---|---|---|---|---|
| 1 | habeas corpus–US | 1 | 2,958 | The federal petitioner: federal prisoner, service member appealing a military court, person resisting extradition, or parolee contesting revocation. **Any** victory counts (see rule A). |
| 2 | habeas corpus–state | 1 | 8,606 | The state prisoner or state-proceedings petitioner. **Any** victory counts (rule A). |
| 3 | criminal court motions | 1 | 15,752 | The criminal defendant bringing the motion (suppress evidence, new trial, and similar, immediately before/during/after trial). |
| 4 | contempt of court | 1 | 56 | **DISCONTINUED** — the 2016 codebook marks this a discontinued category and states no coding rule. Do not emit; see rule D. |
| 5 | (non)conviction–criminal case | 1 | 1,746 | The criminal defendant. Includes government forfeiture of money or goods seized in a criminal investigation — a decision for the defendant/property owner is liberal. |
| 6 | alien petitions | 2 | 3,012 | The alien petitioner (citizenship denied, deportation, redress of grievances against U.S. officials). |
| 7 | Native American rights | 2 | 827 | The Native American individual or the certified tribe. |
| 8 | voting rights | 2 | 1,134 | The citizen asserting a right to vote or to run for office; also all reapportionment and one-person-one-vote claims. A racial-minority plaintiff still codes **here**, not under #10. |
| 9 | Social Security | 3 | 4,243 | The disability-benefits claimant appealing an ALJ denial. Granting benefits is liberal. |
| 10 | racial discrimination | 2 | 5,398 | The racial-minority claimant alleging individual discrimination (Equal Protection or §5 legislation). Includes minority union members suing their own union — **not** #16. |
| 11 | 14th amendment / civil rights acts | 2 | 18,005 | The individual alleging the violation, or the government agency suing on the victim's behalf. Catch-all when a petition mixes pleas that cannot be assigned to a narrower type. Also covers firing from a government job in violation of the 1st Amendment — **not** #13. |
| 12 | military exclusion | 2 | 26 | **DISCONTINUED** — no coding rule stated. Do not emit; see rule D. |
| 13 | freedom of expression | 2 | 4,025 | The person claiming a speech or press violation. Adult-bookstore-vs-zoning disputes code here, not #20. Commercial speech codes here despite its economic component. |
| 14 | freedom of religion | 2 | 1,640 | The person asserting the claim, under **either** the establishment or the free-exercise clause. A finding that an establishment violation exists is liberal; so is a ruling for an employer objecting to the ACA contraception mandate on religious grounds. |
| 15 | union v. company | 3 | 3,300 | The labor union. (Union-vs-union representation disputes are excluded from the database entirely — rule C.) |
| 16 | member v. union | 3 | 1,195 | The individual member or employee suing the union — the underdog, not the union. |
| 17 | employee v. employer | 3 | 9,318 | The employee. Residual labor category when no more precise type (race, gender, disability, age) fits. Includes wrongful-death actions (a win for the widow is liberal), FMLA claims, employee suits against an employer-affiliated insurer, and government-employee-vs-government disputes absent an overt civil rights issue. |
| 18 | commercial regulation | 3 | 6,507 | **The federal regulator.** Suits by independent federal agencies or executive departments to regulate commerce. A victory for the federal regulator — or for the federal government over a would-be state regulator — is liberal. Excludes environmental and pure-food-and-drug matters (#19). |
| 19 | environmental protection | 3 | 3,821 | **The protection interest** — environmental, pure food and drug, or consumer — including suits brought by state or federal regulatory agencies. Coded by *substantive winner, not by which party won*: if an environmental group sues the EPA for inadequate protection and the government loses, the case is **liberal**. A decision favoring government firearm regulation over a 2nd Amendment claim is liberal. OSHA suits by the Secretary of Labor code here, not #24. Excludes consumer suits brought by private individuals against a private company. |
| 20 | local/state economic regulation | 3 | 3,693 | **The state or local government.** A decision upholding state or local economic regulation against challenge (local sales tax, licensing, substantive due process claims) is liberal. |
| 21 | labor dispute–govt v. union/employer | 3 | 1,909 | The Secretary of Labor or the NLRB; also the worker in a Fair Labor Standards Act claim. Mainly pre-1972, splitting into #24 and #25 thereafter, but see rule B — the era is a convention, not a bound. |
| 22 | rent control, excess profits, price control | 3 | 966 | **The government.** A decision upholding federal or state control of rents, corporate profits, or prices is liberal. Concentrated in WWII and the 1970s. |
| 23 | women's rights | 2 | 3,393 | The woman, or an agency such as the EEOC acting on women's behalf. Includes Violence Against Women Act 1994 claims. Excludes abortion (→ #29) and male gender-discrimination claims (→ #11). |
| 24 | NLRB v. employer | 3 | 1,618 | The Secretary of Labor or the NLRB. Includes all Fair Labor Standards Act matters. Excludes OSHA suits (→ #19). Mainly post-1972 (rule B). |
| 25 | NLRB v. union | 3 | 308 | The Secretary of Labor or the NLRB, suing a union or a group of employees. Mainly post-1972 (rule B). |
| 26 | handicapped/disability rights | 2 | 3,789 | The disabled person. Mostly ADA claims against employers, states, or corporations. |
| 27 | reverse discrimination–race | 2 | 221 | **The affirmative-action program.** A decision *upholding* the program is liberal. Individual minority discrimination claims code under #10, not here. |
| 28 | reverse discrimination–sex | 2 | 51 | **The affirmative-action program** giving women priority in hiring or contracting. *Upholding* it is liberal. Women's own discrimination claims code under #23. |
| 29 | right to privacy | 2 | 899 | The person asserting privacy or autonomy: contraception, abortion, gay and lesbian rights, out-of-the-ordinary search and seizure, Privacy Act violations. A ruling that a search was illegal under the 4th Amendment is liberal. **FOIA cases are not coded at all** (rule C). |
| 30 | age discrimination | 2 | 1,702 | The older worker or group of employees. A finding that age discrimination occurred is liberal. |
| 31 | sentencing guidelines deviation | 1 | 859 | The convicted person. Granting a sentence reduction is liberal; so is *denying* a prosecutor's request to extend a sentence. Mainly 2005 onward, but 26% of rows are earlier — see rule B. |

---

## Rules that override the plain reading

**Rule A — "any victory" in criminal and prisoner cases.** For case types 1 and 2, and for
prisoner civil-rights suits against prison authorities wherever they are classified, *any*
victory for the petitioner is coded liberal, however partial. The codebook's stated reason is
variance: the state wins so often that requiring a decisive win would leave no variation in the
dependent variable. This is the district analogue of the paper's Songer Area 1 rule and is the
single most consequential exception in the table.

**Rule B — era conventions, not bounds.** The codebook presents these as firm: case type 21
runs 1933–1972, splitting into 24 and 25 thereafter; case type 31 begins January 2005, with
earlier sentencing matters under type 3.

**The data does not obey them.** Verified against the 2016 release:

| Type | N | Observed year range | Rows outside the stated era |
|---|---|---|---|
| 21 | 1,909 | 1932–2008 | 31 |
| 24 | 1,618 | 1971–2012 | 1 |
| 25 | 308 | 1972–2012 | 0 |
| 31 | 859 | 1997–2012 | **222 (26%)** |

Type 31 is the serious case: a quarter of its rows predate the codebook's own start date.
Treating the era as a hard rule would therefore cause the model to refuse codes the human
coders actually assigned. The prompts state era as guidance about typical usage, and the
post-processor **reports** out-of-era codings without correcting them.

*Exception, added 2026-09-27:* the 21 → 24/25 split at 1972 is one the data does obey. Years
1971/72/73 give 21 54/40/0, 24 1/2/33, and 25 0/1/12. So `03_validate.py` also reports
`ai_casetype_era`: an AI 21/24/25 on an opinion decided through 1972 becomes 21. The raw code is
kept beside it.

**Rule C — categorically excluded subject matter.** Some cases were never entered in the
database at all: union-vs-union representation disputes, FOIA cases, employer-vs-employer
suits, and more broadly any case without a "clear-cut winner" or without what the codebook
calls a "traditional liberal-conservative dimension" (patents, copyrights, land condemnation
are the named examples). If a party wins on two issues and loses on two, the case was not
coded. These feed the `codable` gate in pass 1, not the direction lookup.

**Rule D — discontinued types.** Case types 4 and 12 carry rows (56 and 26) but no coding rule
in the 2016 codebook. They are not offered to the model as options; the post-processor treats
any pilot case whose ground truth is 4 or 12 as out of scope rather than as a model error.

**Rule E — `category` is derived, never coded.** `category` follows deterministically from
`casetype` via the mapping in the Cat column above. In the released data six of 110,977 rows
violate their own mapping (hand-coder slips); the post-processor derives `category` from
`casetype` and reports such rows rather than propagating them.

---

## Super-mechanical variant

Following the paper's best-performing Songer variant (§3.3, p. 14), the post-processor computes
direction twice:

- **Mechanical**: uses the model's reported `focal_prevailed`.
- **Super-mechanical**: discards `focal_prevailed` and recomputes it from the model's
  `disposition` plus `focal_is_movant` — i.e. if the focal interest brought the motion and the
  motion was granted, the focal interest prevailed; if it opposed the motion and the motion was
  denied, likewise. Cases with partial or mixed dispositions yield no direction and are flagged.

Disagreement between the two is a per-case QC signal, exactly as in the paper's Figure 1. Both
columns are written to the output; which one becomes the headline measure is decided from the
pilot's agreement rates, not in advance.
