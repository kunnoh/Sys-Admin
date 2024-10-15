## sed
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


## cut
```sh
cut -d ' ' -f 1 userinfo.txt
```

**Options**
`-d` - delimiter.
`-f` - field.
`-c` - character.

- **Specify Multiple Fields**:
    - `cut -d ' ' -f 1,3 file.txt`: Extracts the 1st and 3rd fields.

- **Range of Fields**:
    - `cut -d ' ' -f 1-3 file.txt`: Extracts fields 1 to 3.
    - `cut -d ' ' -f 2- file.txt`: Extracts from field 2 to the end.

- **Use Different Delimiters**:
    - `cut -d ':' -f 1 file.txt`: Specifies colon (`:`) as the delimiter.
    - `cut -d ',' -f 2 file.txt`: Specifies comma (`,`) as the delimiter.

- **Cut by Character Position**:    
    - `cut -c 1-5 file.txt`: Extracts the first 5 characters from each line.
    - `cut -c 3- file.txt`: Extracts from the 3rd character to the end.

- **Complement Option**:    
    `cut -d ' ' -f 2 --complement file.txt`:
    Displays all fields **except** the 2nd field.

- **Cut from Standard Input**:    
    `echo "one two three" | cut -d ' ' -f 2`:
    Extracts the 2nd field from a string passed through `echo`.

- **Suppress Delimiter in Output**:    
    `cut -d ':' -f 1 --output-delimiter=' ' file.txt`:
    Changes the output delimiter to a space (useful when extracting fields from colon-separated files but need a different delimiter in the result).

- **Cut by Byte**:    
    `cut -b 1-3 file.txt`:
    Extracts the first 3 bytes (this is useful for fixed-width data, but it can behave differently with multi-byte characters).

## uniq


## grep
```sh
grep [options] 'search_pattern' file
```

**Options**
`-i` - case insensitive.
`-r` - search directory recursively.
`--color` -add color to output when using sudo.
`-v` - invert match. words that don't match.
`-w` - match exact search word.
`-o` - show only matching option.

### regex
**Search pattern**
- `'word$'` - $ ends with.
- `'c.t'` - any word that start with c and ends with t with only one character between.

- Check for lines that don't begin with `#` in conf file.
```sh
grep -v '^#' /etc/login.defs
```

- Check line that ends with 7.
```sh
grep '7$' /etc/login.defs
```

- Escape for special characters.
```sh
grep '\.' /etc/login.defs
```

- Match the previous element 0 or more times.
```sh
grep -r 'let*' /etc/
```

