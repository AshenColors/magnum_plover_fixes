# magnum_plover_fixes
A set of transformations to make the Magnum Steno .rtf dictionary work with Plover CAT software better.

TODO: Make a Python script that applies these transformations.

This is only a set of transformations and doesn't include data from the Magnum dictionary itself.

The transformations, as python style regular expressions, are as follows.

```
# Format:
# regular expression string
# replacement string

# DigitalCAT uses ^ alone as a non-breaking space. Change these to actual NBSPs.
r"(?<!\{)\^(?!\})"
"\u00a0"

# Question and Answer formats are translated as bare Q and A characters. Format them as initials.
r"\\n\\nQ"
r"\\n\\nQ. "

r"\{\^\\n\\n\^\}Q"
r"{^\\n\\n^}Q. "

r"\\n\\nA"
r"\\n\\nA. "

r"\{\^\\n\\n\^\}A"
r"{^\\n\\n^}A. "

# Paragraph styles don't exist in Plover. Replace with double newline to be consistent with Q/A.
r"\{P .*?\}"
r"{^\\n\\n^}"

# Plover style is single-spaced. Convert double spaces to single spaces.
r"\u00a0 "
r"\u00a0"

r"  "
r" "
```
