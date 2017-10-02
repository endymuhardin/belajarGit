# Git Workflows

Atau bisa dibilang *cara kerja menggunakan Git*

## Alumni Subversion
### Ciri khas:
- Semua commit dicampur di trunk
- Commit per online, bukan per task
- Branch hanya untuk maintenance rilis

### Cara kerja:
- Clone repo

      $ git clone myrepo

- Hacking / rubah code
- Commit

      $ git commit -am "log message"
      $ git pull

- Resolve conflicts
- Push

      $ git push


### Outcome:
- Commit gak jelas juntrungannya
- Tiap commit tidak bisa di-apply sebagai patch yang solid
- Merge commit di mana-mana

---------

## Git Zealot
### Ciri khas:
- Commit per task
- Bikin branch bahkan untuk ngerjain 1 commit doang
- Rebase melulu

### Cara kerja:
- Clone project lalu buat branch baru

      $ git clone myrepo
      $ git checkout -b topic-branch

- Hack / rubah code
- Pilih hunk yang mau distage

      $ git add -i

- Commit terus Push ke branch baru yang telah dibuat

      $ git commit -m "log message"
      $ git push origin topic-branch

- Jangan gunakan master untuk kerja, master hanya untuk track upstream
- Persiapan untuk rilis

      $ git checkout master
      $ git remote add upstream
      $ git fetch upstream
      $ git merge master upstream/master
      $ git checkout topic-branch

- Pilih:
  - `$ git rebase master` (awas intermediate commit juga harus ditest)
  - `$ git checkout master` dan `$ git merge topic-branch` (jadi ada merge commit) (preferred seperti katanya [nvie](http://nvie.com/posts/a-successful-git-branching-model/))
- Resolve conflicts
- Commit

      $ git commit -m "log message"

- Send pull request

      $ git format-patch

- send untuk review/pull request

### Outcome:
- Clean, linear history
- Patch bisa di-apply secara clean
- Tiap commit jelas urusannya
- Butuh waktu lama untuk pilih2 hunk
