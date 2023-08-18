# magnum_plover_fixes
Below is a set of transformations to make the Magnum Steno .rtf dictionary work with Plover CAT software better.

magnum_fixes.json is a working dictionary of fixes that aren't systematic and can't be applied this way.

commands.json is an original file of mine that provides various Plover and formatting commands and ideally should not conflict too much with Magnum.

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

# Convert DigitalCAT macros.
r" \"(.*?)\{@CAPWORD\(LASTWORD\)\}\""
r" \"{\*-|}\\1\""

# Plover doesn't have a way to 'skip' a space, so SECONDSPACE is identical to FIRSTSPACE instead of replacing the second-to-last hyphen with a space. Requires plover_retro_text_transform
r"\{@HYPHENATE\(FIRSTSPACE\)\}"
r"{:retro_replace_space:1:-}"

r"\{@HYPHENATE\(SECONDSPACE\)\}"
r"{:retro_replace_space:1:-}"

# Plover style is single-spaced. Convert double spaces to single spaces.
r"\u00a0 "
r"\u00a0"

r"  "
r" "

# Trailing spaces result in double-spacing, remove them.
r" \}"
r"}"

r"(?<=: \".*?) \""
r"\""

r" \{-|\}\""
r"{-|}\""
```
