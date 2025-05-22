## Introduction
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

