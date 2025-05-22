## Introduction  
Regex is tool use to find modify string and files name.  

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

