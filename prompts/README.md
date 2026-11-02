# Prompts (version v1)

These are the exact instructions the model received when coding the database. They were frozen
before the production run and are published here byte for byte; the SHA-256 hashes below match
the files the coding run checked before every call.

| File | Role |
|---|---|
| `pass1_system.md` | First call: codability, case type, parties, posture, disposition |
| `pass1_user_template.md` | First call: case metadata and opinion text |
| `pass2_system.md` | Second call: identify the focal party and whether it prevailed |
| `pass2_user_template.md` | Second call: first-pass facts plus the opinion text |
| `focal_roles.json` | The focal-party definition for each case type, inserted into the second call |
| `carp_manning_direction_table.md` | Reference: the codebook's focal party and direction rule for each case type |

Direction is not asked of the model. It is computed afterward: if the focal party prevailed,
the case is coded liberal (1), otherwise conservative (0). A mixed outcome counts as liberal
in the criminal-justice and habeas types (1, 2, 3, 5, 31) and is left missing elsewhere; the
codebook gives the full rule.

`carp_manning_direction_table.md` was written as a working reference during development, and
its file paths refer to the original project, not to this repository.

## Hashes

| File | SHA-256 |
|---|---|
| `pass1_system.md` | `29ba4c00f7223de0c83c9347af4c6fa074ab5db53a5fbc6d7dd3a6eead09b725` |
| `pass1_user_template.md` | `31e20d15105af71599bdd6278885aaa681ddb8c68eafcde32e897a9a77b81a7e` |
| `pass2_system.md` | `bdf62ee252637d56a4fb077e377e4e3130efd34041e53a97a2ec228c6ba94e59` |
| `pass2_user_template.md` | `329163ddb879df4ea4b36af2d4150177295e55901ce715ced42b4d03ec861461` |
| `focal_roles.json` | `de3a4fff57df9ded690e8e6a867f6fbdd318311552bd9774c26e8a5151c48814` |
