## Useful Bash Tidbits

**Display sizes of all directories in the current directory, sorted**
```sh
# linux
# sudo may be unnecessary here
find . -maxdepth 1 -type d  -exec sudo du -sh {} + | sort -r -h
# OSX
find . -maxdepth 1 -type d  -exec du -s {} + | sort -r -n
```

**Sum column n in a huge csv
```sh
# -F, sets comma as delimiter
# gsub(/"/, "", $n) gets rid of quotes that break summing
awk -F, '{ gsub(/"/, "", $n); sum += $n; } END { print sum; }' giant.csv
```
