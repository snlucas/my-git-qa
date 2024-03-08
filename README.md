# My Git QA

<br><br>
## How to get only modified files?
```sh
$ git whatchanged --since="since time" --name-only --oneline | grep -v '^.\{9\}\s' | uniq
```

<br><br>
### Let's say we are in a branch called ba and we have another branch called bb. How to watch for all modified files in bb since 2 weeks ago?
```sh
$ git whatchanged  bb --since="2 weeks ago" --name-only --oneline | grep -v '^.\{9\}\s' | uniq
```

*grep -v* is used to remove lines with sha (9 chars).

<br><br>
### How can I see branch bb modified files in a range:
```sh
$ git whatchanged  bb commitsha1..commitsha2 --name-only --oneline | grep -v '^.\{9\}\s' | sort -u
```

<br><br>
### How to change line breaks to blank spaces in Unix like systems?
```sh
$ awk '{ printf "%s ", $0; } END { printf "\n"; }'
```

*END* in this case has a line break for convenience. It'll only run once, similar to if I had put a *BEGIN*.

<br><br>
## How to remove more than one file using grep?
```sh
$ grep -v -e filea -e fileb -e filec
```

<br><br>
## How to get modified files from a branch called bb?
```sh
$ git checkout bb -- $(git whatchanged bb commitsha1..commitsha2 --no-merges --author="snlucas" --committer="snlucas" --name-only --oneline | grep -v '^.\{9\}\s' | sort -u | awk '{ printf "%s ", $0; } END { printf "\n"; }' | tr '\n' ' ')
```

In this case I get all modified files using git whatchanged and used git checkout in bb branch to get these files. Also, it's important to use **--**, once it tells git that you're dealing with files in this part of the command, helping you avoid some headaches.

In flags --author and --committer change "snlucas" to your Git username.

<br><br>
### Ok, I got some errors with my *$ git checkout -- files*
Sometimes you face some errors with some files. If it's ok, you can avoid these files:
```sh
$ git checkout bb -- $(git whatchanged bb commitsha1..commitsha2 --no-merges --author="snlucas" --committer="snlucas" --name-only --oneline | grep -v '^.\{9\}\s' | sort -u | grep -v -e errorfile1 -e errorfile2 -e errorfile3 | awk '{ printf "%s ", $0; } END { printf "\n"; }' | tr '\n' ' ')
```

<br><br>
## Working with Salesforce and need help to create a package. We can use <a href="https://github.com/scolladon/sfdx-git-delta" targe="_blank">sfdx delta</a>
```sh
$ sfdx sgd:source:delta --to commitsha2 --from commitsha1 --output manifest
```

<br><br>
