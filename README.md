## Useful Bash Tidbits

**Display sizes of all directories in the current directory, sorted**
```sh
# linux
# sudo may be unnecessary here
find . -maxdepth 1 -type d  -exec sudo du -sh {} + | sort -r -h
# OSX
find . -maxdepth 1 -type d  -exec du -s {} + | sort -r -n
```

**Sum column n in a huge csv**
```sh
# -F, sets comma as delimiter
# gsub(/"/, "", $n) gets rid of quotes that break summing
awk -F, '{ gsub(/"/, "", $n); sum += $n; } END { print sum; }' giant.csv
```

**Delete multiline regex from file**
```sh
# Needs tmp file in the middle, because cat $f > $f truncates the file for the > before executing the cat.
f=FILENAME
pcregrep -Mv "some multiline\nregext" $f > tmp.txt
cat tmp.txt > $f

# For many files
for f in $(find ./some/path/* -type f); do
pcregrep -Mv "some multiline\nregext" $f > tmp.txt
cat tmp.txt > $f
done
```
