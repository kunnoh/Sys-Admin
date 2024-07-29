#### Substitute string
Get number of word `About` in the `dista.xml` documents.
```sh
cat /root/dista.xml | grep About | wc -l
```

Replace the word `About` with `ResCF`.
```sh
sed -i -e 's/About/ResCF/g' /root/dista.xml
```

