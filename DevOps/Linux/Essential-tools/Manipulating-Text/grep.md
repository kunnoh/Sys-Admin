## Introduction
Grep is a tool used to search strings.  

## Application
General commands.
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
`-E` - Use extended regex


### Show uncomment config lines
```sh
grep -vE '^\s*#|^\s*$' /etc/ssh/sshd_config --color
```  
Breakdown:
- `-v` invert match, i.e., exclude lines that match the pattern.
- `-E` use extended regex.
- `'^\s*#|^\s*$'` regex pattern that matches:
    - `^\s*#`: lines that start with optional whitespace and then a # (comment lines).
    - `^\s*$`: lines that are completely empty or just whitespace.  

So this part removes:
- All comments starting with '#'.
- All blank lines.


## Referances
1. [Grep wiki](https://en.wikipedia.org/wiki/Grep)
2. []()