## Introduction
Sed is used to search and modify string.  


**Substitute string**
Get number of word `About` in the `dista.xml` documents.
```sh
cat /root/dista.xml | grep About | wc -l
```

Replace the word `About` with `ResCF`.
```sh
sed -i -e 's/About/ResCF/g' /root/dista.xml
```

`'s/About/ResCF/g'` 
- `s` - search and replace.
- `About` - word to search.
- `ResCF` - word to replace with.
- `g` - should be global search and replace.

**Options**
- `-i` or `--in-place`.
- `-e` 

- **Make global and case-insensitive**
	you would need both `g` (global) and `I` (case-insensitive):
```sh
sed 's/About/ResCF/gI' /root/dista.xml
```

- **Substitute With a Regex Pattern**:
	`sed 's/[0-9]/#/g' file.txt` 
	Replaces all digits with `#`.
	
- **Replace Only on Specific Line(s)**:
	`sed '3s/old/new/' file.txt`
	Replaces "old" with "new" only on line 3.

- **Print Matching Lines**:
	`sed -n '/pattern/p' file.txt`
	Only prints lines that match the given pattern.

- **Replace Only on Specific Line(s)**:
	`sed '3s/old/new/' file.txt`
	Replaces "old" with "new" only on line 3.

- **Insert/Append Lines**:
	`sed '2i\This is a new line' file.txt`:
	Inserts "This is a new line" before line 2.
	
	`sed '2a\This is a new line' file.txt`
	Appends "This is a new line" after line 2.

