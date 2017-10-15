# Langkah Migrasi dari Subversion ke Git

1. Dump repository Subversion
    ```bash
    svnadmin dump /path/ke/repo/svn > nama-file-dump
    ```

2. Copy ke PC kita sendiri supaya cepat prosesnya

3. Restore lagi
    ```bash
    svnadmin load /path/ke/repo/svn < nama-file-dump
    ```

4. Checkout
    ```bash
    svn co file:///path/ke/repo/svn checkout-project-svn
    ```

5. Generate authors file
    ```bash
    #!/usr/bin/env bash
    authors=$(svn log -q | grep -e '^r' | awk 'BEGIN { FS = "|" } ; { print $2 }' | sort | uniq)
    for author in ${authors}; do
      echo "${author} = NAME <USER@DOMAIN>";
    done
    ```
    Simpan hasilnya ke `file-authors.txt`. Ini akan kita gunakan di langkah selanjutnya. 

6. Checkout lagi menggunakan `git-svn`
    ```bash
    git svn clone --stdlayout --no-metadata -A file-authors.txt file:///path/ke/repo git-svn-migrasi-project
    ```

7. Masuk ke folder hasil clone
    ```bash
    cd git-svn-migrasi-project
    ```

8. Konversi branch svn menjadi branch git
    ```bash
    git branch -r | grep -v tags | sed -rne 's, *([^@]+)$,\1,p' | while read branch; do echo "git branch $branch $branch"; done | sh
    ```

9. Konversi tag svn menjadi tag git
    ```bash
    git branch -r | sed -rne 's, *tags/([^@]+)$,\1,p' | while read tag; do echo "git tag $tag 'tags/${tag}^'; git branch -r -d tags/$tag"; done | sh
    ```

10. Konversi `svn-ignore` menjadi `gitignore`
    ```bash
    git svn show-ignore > .gitignore
    git add .gitignore
    git commit -m "konversi svn ignore menjadi gitignore"
    ```

11. Clone supaya bersih
    ```bash
    cd ..
    git clone --bare git-svn-migrasi-project nama-project.git
    ```

12. Copy ke server untuk di-share menggunakan Gitosis




