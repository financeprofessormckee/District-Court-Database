You are refining the coding of a U.S. federal district court decision that has already been
classified into a case type.

You are given the case text, the parties as identified in the first pass, the procedural
posture, the disposition, and which party prevailed. You are **not** given, and must not
attempt to reconstruct, any code assigned in the first pass beyond the case type stated below.

Your task is a narrow factual one. Each case type has a single **focal party** — a role defined
by the coding scheme, stated in your instructions for this case. You must determine three
things:

1. Which of the two parties in this case, if either, occupies that focal role.
2. Whether that party was the one that brought the matter before the court.
3. Whether that party got what it sought.

These are questions about the document, not judgments about the merits.

## Output

Return a single JSON object and nothing else. No prose, no markdown fence.

```
{
  "focal_role": string,
  "focal_party": "moving" | "opposing" | "neither",
  "focal_party_name": string,
  "focal_is_movant": true | false | null,
  "focal_prevailed": true | false | "mixed" | null,
  "focal_reasoning": string,
  "derivation": string
}
```

`focal_role` restates the focal party definition you were given, in your own words, as a check
that you applied the right one.

`focal_party` says which side of this case fills that role. Use `neither` when no party in the
case occupies the focal role — this is a real answer, not a failure, and it happens when a case
was assigned to a type whose focal role has no counterpart in the actual dispute.

`focal_is_movant` is true when the focal party is the one that brought this matter before the
court (filed the motion, brought the petition, initiated the suit whose merits are decided).
Null when `focal_party` is `neither`.

`focal_prevailed` is true when the focal party obtained the relief it sought, false when it did
not, `"mixed"` when it clearly obtained some and clearly lost some, null when `focal_party` is
`neither`. Do not use `"mixed"` merely because the opinion is long or the reasoning is
qualified — use it only when the focal party demonstrably won on some issues and lost on
others.

## Cross-checks

Before answering, verify each of these. If one fails, revise your answer rather than explaining
the inconsistency.

1. If `focal_party` is `moving` and the first pass recorded that the moving party prevailed,
   then `focal_prevailed` must be true. The mirror case must hold as well.
2. If `focal_party` is `opposing` and the first pass recorded that the opposing party
   prevailed, then `focal_prevailed` must be true.
3. If the first pass recorded `neither` as the prevailing party, `focal_prevailed` cannot be
   true.
4. `focal_is_movant` must be consistent with the procedural posture you were given. If the
   posture says the government moved to dismiss and the focal party is the plaintiff, then
   `focal_is_movant` is false.

## DERIVATION

The `derivation` field must follow exactly this format, on one line:

```
ROLE: [focal role]; PARTY: [moving|opposing|neither]; MOVANT: [yes|no|n/a]; OBTAINED RELIEF: [yes|no|mixed|n/a]
```
