# Menggunakan Git dengan SVN

## Perbedaan arsitektur dengan svn
- **SVN**  
  working &rarr; remote repo
- **Git**  
  working &rarr; staging &rarr; local repo &rarr; remote repo

## Istilah
- HEAD: posisi branch saat ini
- trunk: branch utama di svn
- master: branch utama di git
- commit
    - svn: memindahkan changes dari working ke remote
    - git: memindahkan changes dari staging ke local repo
- checkout
    - svn: memindahkan changes dari remote ke working (git: git pull)
    - git: pindah ke branch lain (svn: svn switch)
- add
    - svn: menambahkan file yang belum diversion
    - git: menambahkan changes ke staging
- rm
    - svn & git: stop versioning file, hapus dari filesystem


## Workflow

1. Instalasi
    ```
    apt-get install git-svn
    ```

2. Checkout svn repo
    ```
    git svn clone -s http://blabla 
    ```

3. Konfigurasi dulu user dan email
    ```
    git config --global user.name "Endy Muhardin"
    git config --global user.email "endy@artivisi.com"
    ```

    Biar ada warnanya
    ```
    git config --global color.ui true
    ```

4. Import `svn:ignore` ke `.gitignore`
    ```
    git svn show-ignore > .gitignore
    ```

4. Best practices, kerjanya di topic branch, jangan di master
    ```
    git checkout -b branch_kerja
    ```
    ini cara cepat, cara lambatnya: 
    ```
    git branch branch_kerja
    git checkout branch_kerja
    ```

    Lihat daftar branch, yang ada bintangnya berarti kita sedang berada di sana
    ```
    git branch
    ```

5. Lihat status working folder
    ```
    git status -s
    ```

6. Add modifikasi ke staging index
    ```
    git add . 
    ```
    Perintah add akan mendaftarkan *changeset* ke staging. **Bukan** file!!!
    Jadi kalau kita edit file `halo.txt`, add, kemudian edit lagi, 
    maka yang akan berangkat pada saat commit hanya perubahan pertama. 
 

7. Commit ke local repo
    Yang ini hanya commit yang sudah ada di staging index
    ```
    git commit -m "keterangan commit"
    ```
    Kalo males add dulu baru commit, langsung aja commit dari working folder ke local repo
    ```
    git commit -a -m "keterangan commit"
    ```

8. Melihat keterangan commit terakhir (-1)
    ```
    git log -1
    ```

    Lengkap dengan diff nya
    ```
    git log -p -1
    ```

9. Pindah lagi ke master (branch utama)
    ```
    git checkout master
    ```

10. Update repo lokal terlebih dahulu, mungkin saja ada yang sudah commit sementara kita sibuk
    ```
    git svn rebase
    git fetch origin + git merge origin/master
    ```

11. Apply semua commit di branch_kerja ke master
    ```
    git merge --no-ff --no-commit branch_kerja
    ```
    `no-ff`: semua commit gabung jadi satu, di-commit lagi di branch tujuan  
    `no-commit`: jangan langsung commit, biarkan saja di staging index, supaya kita bisa membuat commit message sendiri biasanya berisi release note  
    `squash`: semua commit digabung jadi 1  

    Kadang-kadang terjadi fast forward kalau di master tidak ada commit, sehingga historinya menjadi linier  
    Kalau master tidak ada commitnya, gunakan opsi `--no-ff` untuk menandai commit
    ```
    git merge --squash --no-ff branch_kerja
    ```

    Fast forward adalah pemindahan pointer HEAD branch target menjadi sama seperti branch asal.  
    Merge fast forward tidak menimbulkan commit baru

12. Cek apakah ada konflik.
    Kalau ada, fix terlebih dahulu, baru commit. 
    Kalau tidak ada, ya langsung saja commit
    ```
    git add .
    git commit -m "implement task #12"
    ```

13. Sudah oke semua, commit ke svn repo
    ```
    git svn dcommit
    ```
    1 commit di master = 1 commit di svn 

14. Cara undo
    - Skenario 1: masih di local dan belum naik ke staging (belum add), mau kembalikan ke posisi terakhir commit
        ```
        git reset --hard HEAD 
        ```
    - Skenario 2: sudah add tapi belum commit, add-nya mau di-cancel
        ```
        git reset -- namafile
        ```

    - Skenario 3: sudah add tapi belum commit, setelah add ada modifikasi lagi, mau dikembalikan ke kondisi terakhir sync
        ```
        git reset --hard 
        ```

    - Skenario 4: sudah add tapi belum commit, setelah add ada modifikasi lagi, mau dikembalikan ke kondisi terakhir add
        ```
        git checkout -- namafile 
        ```

    - Skenario 5: sudah commit, tapi belum push / dcommit
        ```
        git reset HEAD^ # kembalikan ke commit - 1
        git reset HEAD~3 # hapus commit terakhir, commit sebelumnya, dan commit sebelumnya lagi
        ```
        `-- soft`: cuma HEAD yang direset, working dan index tetap  
        `-- mixed`: index dan HEAD direset, working tidak  
        `-- hard`: working, index, dan HEAD direset semua

    - Skenario 6: sudah push 
        ```
        git revert [sha_commit_yang_mau_diundo] # akan menggenerate commit baru yang isinya reverse diff dari commit yang direvert
        ```

15. Interrupted workflow
Misalnya, sedang asyik tambah fitur, ada request bugfix

    - pindahkan work in progress ke branch baru
        ```
        git stash fitur-baru
        git commit -m "save dulu sebentar, nanti abis bugfix diterusin lagi"
        ```

    - balik lagi ke master, kali ini sudah bersih dari tambahan fitur, seperti kondisi terakhir commit
        ```
        git checkout master
        ```

    - add bugfix, lalu commit
        ```
        git commit -m "bugfix"
        ```

    - balik ke branch fitur-baru, terserah mau rebase atau merge
        ```
        git checkout fitur-baru
        git rebase master / git merge master
        ```
        ... edit ... edit ... 
        ```
        git commit -m "fitur baru selesai"
        ```

    - integrate ke master
        ```
        git checkout master
        git merge fitur-baru
        ```

## Bekerja dengan remote
Skema URL: 
- Local: `/path/ke/repo`
- SSH: `username@hostname:/path/ke/repo`
- GIT: `git://hostname/namarepo`
- HTTP(s): `http(s)://hostname/namarepo`

1. Kalau di lokal belum ada reponya, clone saja
    ```
    git clone namaremote urlremote
    ```

2. Kalau di lokal ada, dan mau sync dengan repo orang lain, add remote saja
    ```
    git remote add namaremote urlremote
    ```

3. Ambil changeset dari remote
    ```
    git fetch namaremote
    ```

untuk menghapus juga branch yang sudah tidak ada di remote, gunakan 
opsi `--prune`
    ```
    git fetch --prune namaremote
    ```

4. Setelah di-fetch, merge ke local supaya bisa diedit
    ```
    git merge 
    ```

4. Tracking branch, branch di local sebagai padanan remote
Manual
    ```
    git branch localbranch namaremote/remotebranch
    git checkout localbranch
    ```

Otomatis checkout
    ```
    git checkout -b localbranch namaremote/remotebranch
    ```

Kalau namanya sama (localbranch = remotebranch), bisa pakai `--track`
    ```
    git checkout --track namaremote/remotebranch
    ```

5. Push commits ke remote
    ```
    git push namaremote localbranch:remotebranch
    ```

    Kalau nama branch di remote sama dengan local
    ```
    git push namaremote namabranch
    ```

    Kalau dikosongkan, git akan push ke remote branch yang namanya sama dengan local  
    misalnya master &rarr; namaremote/master
    ```
    git push namaremote
    ```

6. Menghapus branch di remote
    ```
    git push namaremote :remotebranch
    ```


## Fine tuning commit
Misalnya ada beberapa perubahan di 1 file untuk menyelesaikan 3 issue. 
Kita ingin menyesuaikan commit dengan issue, sehingga untuk 1 file ini commitnya harus dipecah. 
Caranya, kelompokkan change dengan menggunakan staging area. 
Kita sortir per potongan file (hunk)

```
git add -i namafile
```

Nanti kita akan ditunjukkan diff dari file. Kemudian bisa: 
- `update`: add ke staging
- `patch`: pilih satu-satu change yang mau distaging
- `revert`: gak jadi staging
- `status`: melihat rekapan semua file yang berubah di lokal dan di staging

Diff dan Patch
Kalau kita punya diff, kita bisa mengubah bentuk suatu file dengan patch
Misalnya awal + diff = akhir

```
patch awal.txt < diff.txt
```

Mengembalikan akhir ke awal

```
patch -R akhir.txt < diff.txt 
```

## Gitosis

Add User: 
- add pubkey ke folder keydir
- add di konfig gitosis.conf
- commit
- push

Add Project: 
- siapkan repo lokal
- `git add remote namaremote git@server:namarepo`
- `git push namaremote master`


## Debugging
1. Switch working folder ke revisi lama
    ```
    git checkout namacommit
    ```

2. Kembalikan lagi ke revisi terakhir
    ```
    git reset --hard
    ```

3. Lihat siapa yang berbuat (blame)
    ```
    git blame -L 2405,2418 namafile
    ```

4. Menelusuri bug dengan bisect
    ```
    git bisect start
    git bisect bad
    git bisect rev-terakhir-yang-masih-bagus
    ```

- git akan switch ke pertengahan rev-bagus dan current
- cek file yang menimbulkan bug, atau test aplikasinya
- kalau bagus, ketik git bisect good
- kalau broken, ketik git bisect bad
- teruskan sampai ketemu pesan 
`a25f13e5e924e55752fd39f7164627ce822ba699 is the first bad commit`
- itulah commit yang menyebabkan error
- kembalikan ke posisi awal
    ```
    git bisect reset
    ```

## Releasing
- Local Tag
    ```
    git tag -a 01.00.00.GA -F CHANGE.txt
    ```

- Menambah tag di tag existing (promote RC to stable)
    ```
    git tag -a 01.00.00 a25f13e
    ```

- Informasi tag
    ```
    git show 01.00.00.GA
    ```

- Menghapus tag
    ```
    git tag -d 01.00.00.GA
    ```

- Push Tag ke Origin
    ```
    git push origin --tags # semua tags
    git push origin 01.00.00.GA # hanya 1 tag tertentu
    ```

- Lihat perubahan sejak tagging terakhir: 
    ```
    git log --pretty=format:'%ad - %an - %h - %s' 02.01.00.M002..HEAD
    ```

- Lihat daftar file yang berubah sejak tagging terakhir
    ```
    git log --name-status 02.01.00.M002..HEAD
    ```

- Output ke file supaya bisa diedit untuk release note
    ```
    git --no-pager log --pretty=format:'%h - %s' 02.01.00.M002..HEAD > output.txt
    ```

- Output ke file untuk menentukan daftar file untuk incremental deployment
    ```
    git --no-pager log --name-status 02.01.00.M002..HEAD > daftar-file-deploy.txt
    ```

## Konversi repo
1. Checkout dulu repo subversion
    ```
    svn co file:///repo/yang/mau/dimigrasi 
    ```

2. Generate svnusers
    ```
    #!/usr/bin/env bash
    authors=$(svn log -q | grep -e '^r' | awk 'BEGIN { FS = "|" } ; { print $2 }' | sort | uniq)
    for author in ${authors}; do
      echo "${author} = NAME <USER@DOMAIN>";
    done
    ```

3. Checkout lagi menggunakan git-svn
    ```
    mkdir migrasi-git
    cd migrasi-git
    git svn clone --stdlayout --no-metadata -A file-authors.txt file:///repo/yang/mau/dimigrasi/trunk dest_dir-tmp
    ```

4. Migrasi branch svn menjadi branch git
    ```
    $ git branch -r | grep -v tags | sed -rne 's, *([^@]+)$,\1,p' | while read branch; do echo "git branch $branch $branch"; done | sh
    ```

5. Migrasi tag svn menjadi tag git
    ```
    $ git branch -r | sed -rne 's, *tags/([^@]+)$,\1,p' | while read tag; do echo "git tag $tag 'tags/${tag}^'; git branch -r -d tags/$tag"; done | sh
    ```

4. Konversi `svn-ignore` menjadi `gitignore`
    ```
    git svn show-ignore > .gitignore
    git add .gitignore
    git commit -m "konversi svn ignore menjadi gitignore"
    ```

5. Clone supaya bersih
    ```
    cd ..
    git clone --bare migrasi-git migrasi-git-bersih
    ```

6. Pasang di server
    ```
    scp migrasi-git-bersih ke server
    ```

7. Mengganti committer name setelah clone
    ```
    git-filter-branch --env-filter "export GIT_AUTHOR_NAME='New name'; export GIT_AUTHOR_EMAIL='New email'" HEAD
    ```

### Sumber
[Creating a svn.authorsfile when migrating from subversion to git](http://technicalpickles.com/posts/creating-a-svn-authorsfile-when-migrating-from-subversion-to-git/)  
[Cleanly Migrate Your Subversion Repository To a GIT Repository](http://www.jonmaddox.com/2008/03/05/cleanly-migrate-your-subversion-repository-to-a-git-repository/)  
[How to migrate SVN repository with history to a new Git repository?](http://stackoverflow.com/questions/79165/how-to-migrate-svn-with-history-to-a-new-git-repository)  
[SVN to GIT migration](http://jausoft.com/blog/2009/07/08/svn-to-git-migration-1/)  
[Migrating from svn to git](http://blog.zobie.com/2008/12/migrating-from-svn-to-git/)
