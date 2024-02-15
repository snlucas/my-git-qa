# My Git QA

<br><br>
## Look For Modified Files
```sh
$ git whatchanged --since="since time" --name-only --oneline | grep -v '^.\{9\}\s' | uniq
```

<br><br>
### Let's say we are in a branch called ba and we have another branch called bb. To watch for all modified files in bb since 2 weeks ago, we can do so:
```sh
$ git whatchanged  bb --since="2 weeks ago" --name-only --oneline | grep -v '^.\{9\}\s' | uniq
```

<br><br>
### If I want to see branch bb modified files in a range:
```sh
$ git whatchanged  bb commitsha1..commitsha2 --name-only --oneline | grep -v '^.\{9\}\s' | uniq
```

<br><br>
### To change line breaks to blank spaces in Unix like systems we can use:
```sh
$ awk '{ printf "%s ", $0; } END { printf "\n"; }'
```

*END* in this case has a line break for convenience. It'll only run once, similar to if I had put a *BEGIN*.

<br><br>
## Remove more than one file using grep
```sh
$ grep -v -e filea -e fileb -e filec
```

<br><br>
## Get modified files from a branch called bb
```sh
$ git checkout bb -- $(git whatchanged bb commitsha1..commitsha2 --no-merges --author="snlucas" --committer="snlucas" --name-only --oneline | grep -v '^.\{9\}\s' | uniq | awk '{ printf "%s ", $0; } END { printf "\n"; }' | tr '\n' ' ')
```

In this case I get all modified files using git whatchanged and used git checkout in bb branch to get these files. Also, it's important to use **--**, once it tells git that you're dealing with files in this part of the command, helping you avoid some headaches.

<br><br>
### Ok, I got some errors with my *$ git checkout -- files*
Sometimes you face some errors with some files. If it's ok, you can avoid these files:
```sh
$ git checkout bb -- $(git whatchanged bb commitsha1..commitsha2 --no-merges --author="snlucas" --committer="snlucas" --name-only --oneline | grep -v '^.\{9\}\s' | uniq | grep -v -e errorfile1 -e errorfile2 -e errorfile3 | awk '{ printf "%s ", $0; } END { printf "\n"; }' | tr '\n' ' ')
```

<br><br>
## If you work with Salesforce and need help to create your package you can use <a href="https://github.com/scolladon/sfdx-git-delta" targe="_blank">sfdx delta</a>
```sh
$ sfdx sgd:source:delta --to commitsha2 --from commitsha1 --output manifest
```

<br><br>
