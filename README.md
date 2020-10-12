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

**Rewrite Git Author**  
From this article: https://docs.github.com/en/enterprise/2.17/user/github/using-git/changing-author-info
```sh
git clone --bare https://hostname/user/repo.git
cd repo.git
```
Build the following script file `replace_git_author.sh`, RELACE EXAMPLE EMAILS
```sh
#!/bin/sh

git filter-branch --env-filter '

OLD_EMAIL="your-old-email@example.com"
CORRECT_NAME="Your Correct Name"
CORRECT_EMAIL="your-correct-email@example.com"

if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
export GIT_COMMITTER_NAME="$CORRECT_NAME"
export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
export GIT_AUTHOR_NAME="$CORRECT_NAME"
export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```
Run the script
```sh
chmod +x replace_git_author.sh
sh replace_git_author.sh
```
Push changes
```sh
git push --force --tags origin 'refs/heads/*'
```
Clean up the temporary clone:
```sh
cd ..
rm -rf repo.git
```
