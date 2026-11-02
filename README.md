# U.S. District Court Database, 2013–2019 Extension

The Carp–Manning U.S. District Court Database records the issue area and ideological
direction of federal trial-court decisions published in the *Federal Supplement*, but its
hand coding stops in 2012. This repository extends it through 2019. It covers all 44,275
*Federal Supplement* opinions in CourtListener decided from 2013 through 2019, coded with the
Carp–Manning case types and liberal/conservative direction.

The codes were produced by a language model working from the Carp–Manning codebook, following
the approach of Sapiro-Gheiler and Kastellec (2026) for the Courts of Appeals. The model
extracts facts from each opinion: the parties, the outcome, which party prevailed, and the case
type. Direction is then computed from those facts with the codebook's own rules, not asked of
the model directly.

Current release: **0.3.1** (see [CHANGELOG.md](CHANGELOG.md)).

## Paper

Eric McKee, Anne Macy, and Wei Zhang. "Extending the U.S. District Court Database Past 2012
with a Validated Language-Model Coder." Working paper.

SSRN link: *coming soon.* The paper describes how the data were built and validated. Please
cite it if you use the data; [CITATION.cff](CITATION.cff) has the details, and GitHub's
"Cite this repository" button reads from it.

## What's in the data

One row per CourtListener opinion cluster (`cluster_id`, unique), 66 variables.

| | records |
|---|---|
| All opinions in the file | 44,275 |
| District-court opinions (`in_district_analysis`) | 43,090 |
| District opinions the coder judged codable | 23,027 |
| District opinions with a direction (`libcon_mechanical`) | 18,691 |

The 1,185 non-district records are Court of International Trade, Judicial Panel on
Multidistrict Litigation, and a few other courts whose opinions appear in the *Federal
Supplement*. They are kept in the file and flagged, not dropped.

Variables fall into four groups:

- **Case identifiers**: CourtListener cluster and docket ids, case name, court, decision date,
  reporter volume and page, and CourtListener's nature-of-suit code.
- **Coding**: the codability judgment and its stated reason, case type (the 31 Carp–Manning
  types) and broad category, disposition, the parties, the focal party for the case type, who
  prevailed, and the model's short written reasoning.
- **Direction**: `libcon_mechanical` (1 liberal, 0 conservative) and the stricter
  `libcon_super_mechanical`, both computed from the coded facts.
- **Judges**: the authoring judge matched to CourtListener and Federal Judicial Center
  identifiers, appointing president and party, and judge attributes in Carp–Manning's own
  coding (`cm_*`), so the extension can be stacked with the original database.

The full opinion text is not included; every row links back to CourtListener by `cluster_id`.

The [codebook](docs/district_court_extension_codebook.pdf) defines every variable, value label,
and missing-value rule. Read it before analysis, especially the sections on codability and on
judge matching.

### Why it ends in 2019

The last decision is dated 2019-09-09. That is a limit of the source, not of the coding:
CourtListener has not yet attached *F. Supp. 3d* citations to most opinions from late 2019
onward, so they cannot be identified as published *Federal Supplement* decisions.

## How accurate are the codes?

The coder was scored against 1,500 hand-coded Carp–Manning opinions, drawn as a stratified
random sample and weighted back to the database's composition. Agreement was:

| | agreement |
|---|---|
| Case type (31 categories) | 79.5% |
| Category (criminal, civil rights and liberties, economic) | 92.5% |
| Direction (liberal/conservative) | 93.2% |

As a check on substance, Democratic appointees in the extension decide liberally 7.5 points
more often than Republican appointees. In Carp–Manning's last eight years (2005–2012) the gap
is 8.5 points.

Two cautions:

1. **About 47% of opinions are coded uncodable**, compared with roughly 9% in the validation
   sample. That is expected: Carp–Manning itself never entered cases without a clear winner or
   a liberal–conservative dimension, so every validation case was codable by construction. The
   `codable_reason` field says why each case was set aside.
2. **The validation measures how often the coder agrees with human coders on cases the humans
   chose to code.** How often it codes a case the humans would have left out has not yet been
   measured. The paper discusses what this means for analysis.

## Quick start

All three examples select district opinions that pass the codability gate and have a direction.
In the CSV, true/false flags are the strings `True` and `False`; in the Stata file they are
labeled 1/0.

**Python**

```python
import pandas as pd

x = pd.read_csv("data/district_ai_codes_2013_2019.csv", dtype=str, keep_default_na=False)
assert len(x) == 44275 and x.cluster_id.is_unique

d = x.loc[(x.in_district_analysis == "True")
          & (x.codable == "True")
          & (x.libcon_mechanical != "")].copy()
assert len(d) == 18691
d["direction"] = pd.to_numeric(d.libcon_mechanical)
print(d.direction.mean())
```

**R**

```r
x <- read.csv("data/district_ai_codes_2013_2019.csv",
              colClasses = "character", na.strings = NULL, check.names = FALSE)
stopifnot(nrow(x) == 44275L, !anyDuplicated(x$cluster_id))

d <- subset(x, in_district_analysis == "True" & codable == "True" & libcon_mechanical != "")
stopifnot(nrow(d) == 18691L)
mean(as.numeric(d$libcon_mechanical))
```

**Stata (14 or later)**

```stata
use "data/district_ai_codes_2013_2019.dta", clear
isid cluster_id
keep if in_district_analysis == 1 & codable == 1 & !missing(libcon_mechanical)
assert _N == 18691
summarize libcon_mechanical
```

## Files

| Path | Contents |
|---|---|
| `data/district_ai_codes_2013_2019.csv` | The database, CSV (UTF-8) |
| `data/district_ai_codes_2013_2019.dta` | The same data in Stata format, with value labels |
| `data/district_ai_codes_2013_2019_dictionary.json` | Machine-readable variable dictionary and code mappings |
| `data/district_ai_codes_2013_2019_manifest.json` | Release version, row counts, and checksums |
| `data/SHA256SUMS.txt` | SHA-256 checksums for the two data files |
| `docs/district_court_extension_codebook.pdf` | Codebook |
| `docs/paper/` | Working paper (SSRN link to follow) |
| `prompts/` | The exact model instructions used to code every opinion |

The CSV and Stata files hold the same values. The codebook documents the few places where their
representations differ, such as dates and labeled categories.

## How the coding was done

Each opinion went through two model calls, using OpenAI's GPT-5.6 Luna with prompt version v1.
The first call decides whether the case is codable and extracts the case type, parties,
posture, and outcome. The second call is given the case type and identifies the codebook's
focal party and whether that party prevailed. Direction then follows mechanically: a win for
the focal party is liberal, a loss is conservative. The prompts were frozen before production
and are published in [`prompts/`](prompts/) unchanged.

The model saw only the opinion and its caption metadata, nothing published after the decision.

## Sources

- **Opinions, dockets, and judge records:** CourtListener, Free Law Project,
  <https://www.courtlistener.com/>.
- **Codebook and comparison data:** Robert A. Carp and Kenneth L. Manning, U.S. District Court
  Database, <http://districtcourtdatabase.org>.
- **Judge identifiers:** Federal Judicial Center, Biographical Directory of Article III Federal
  Judges.
- **Method:** Eitan Sapiro-Gheiler and Jonathan P. Kastellec, "Appealing to
  Large-Language-Model-as-Judge: A Comprehensive Machine-Coded Database for the U.S. Courts of
  Appeals," working paper, 2026.

If you use the data, please also cite Carp and Manning. The extension follows their codebook
and is meant to be used alongside their database.

## License

The data and documentation (`data/`, `docs/`) are released under the
[Creative Commons Attribution 4.0 International license](LICENSE-DATA.md). The prompts and any
code are under the [MIT license](LICENSE).

## Contact

Questions, corrections, and reports of miscoded cases are welcome. Open an issue, or write to
Eric McKee (emckee@wtamu.edu), Paul and Virginia Engler College of Business, West Texas A&M
University.
