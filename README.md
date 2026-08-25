# MBox.Tools
Tools for working with mbox files

## Background

- Forced to switch from email program alpine to evolution
- Need to be able to export messages and attachments in a simple way (i.e. .txt files)

## mbox2txt
- Original code from:  adamhooper/mbox_to_overview_folder.py on 18 Oct 2021
- Modified to
  - Use sender, date, and subject for file name
  - Save attachments into a separate folder
  - Ensure file and folder names are unique
  - Prints names of files being saved
  - Asks if you want to remove mbox if all is successful

### File name sanitizing

Names come from the Subject line, so whatever a correspondent typed ends up
on disk.  `simplify_filename()` therefore:

- Drops the characters no filesystem should be asked to store -- `< > : " / \
  | ? *` and the C0 control range.  Subjects are full of them: `:` from "Re:"
  and "CC:", `"` around display names, `< >` around addresses.  On ext4 these
  merely have to be quoted in every shell command; NTFS refuses them outright.
- Replaces spaces, tabs and newlines with periods, and collapses runs of
  periods to one.
- Strips a trailing period or space, which NTFS cannot store.
- Prefixes an underscore to the Windows reserved stems (`CON`, `PRN`, `AUX`,
  `NUL`, `COM1`-`COM9`, `LPT1`-`LPT9`), which are refused even with an
  extension.
- Caps the name at 180 bytes, under ext4's 255-byte limit for a single path
  component, leaving room for the extension and the uniquifying suffix.
- Returns `untitled` for an unnamed attachment rather than raising.

Non-ASCII is left alone on purpose: it is legal everywhere and carries
meaning.  The rule is to DROP an offending character and keep the rest
verbatim, not to substitute other punctuation.
