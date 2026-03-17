# Remove all underscores

This script renames files within a folder and its subfolders by replacing all underscores (`_`) with spaces.

## Instructions

Open Terminal, navigate to the target folder using `cd folder_name` (or drag and drop the folder into Terminal, if supported), press Enter, then paste and run the following code:

```bash
find . -depth -type f ! -name "._*" | while IFS= read -r f; do
b="${f##*/}"
d="${f%/*}"
nb="${b//_/ }"
[ "$b" = "$nb" ] && continue
target="$d/$nb"
if [ -e "$target" ]; then
printf "[SKIP] Target already exists: \"%s\" -> \"%s\"\n" "$f" "$target" | tee -a rename_errors.log
continue
fi
if ! mv -- "$f" "$target"; then
printf "[ERROR] Unable to rename \"%s\" -> \"%s\"\n" "$f" "$target" | tee -a rename_errors.log
fi
done
