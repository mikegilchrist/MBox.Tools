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
