# Berkontribusi di Proyek git

----
## Latar belakang
cek [Pondokprogrammer](pondokprogrammer.com/tutorial-github/).

Mungkin sudah terlalu sering kita bekerja mandiri pada project pribadi. Di sini, saya akan menunjukkan bagaimana caranya berkontribusi di project lain. Hal termudah untuk mencapai itu adalah dengan mencari proyek open source. Proyek perangkat lunak open source merupakan proyek yang memberikan kode program kepada penggunanya secara bebas, dan tak jarang pengembangannya dilakukan secara terbuka; siapa pun boleh berkontribusi dalam menulis kode tersebut.

Menggunakan perangkat lunak open source tentunya sangat baik, karena selain tidak melakukan pembajakan, kita juga mendukung para pengembang dari perangkat lunak yang kita gunakan. Tetapi, akan lebih baik lagi jika kita juga ikut berkontribusi, mulai dari kontribusi penggunaan, pelaporan bug, sampai dengan kontribusi kode. Kontribusi pada proyek open source akan membantu kita untuk meningkatkan kemampuan pengembangan perangkat lunak."

----
## Bagaimana

Berikut langkah-langkahnya secara singkat:

1. Fork it!
2. Buatlah *branch* fitur baru: `git checkout -b my-new-feature`
3. *Commit* perubahannya: `git commit -am 'Add some features'`
4. *Push* ke branch di *remote*: `git push origin my-new-feature`
5. Buat *pull request*

1. Cari proyek open source.  
*Kali ini, saya sebagai pengembang Android akan menggunakan* **[Material Tabs](https://github.com/neokree/MaterialTabs)** *sebagai contoh.*
2. Cari info tentang aturan kontribusi, atau hubungi developer yang terkait baik via email atau media sosial.
3. Jika memang tidak tertera aturan kontribusi dan sang developer tidak merespons, Anda bisa langsung melakukan fork proyek yang akan Anda kontribusikan.
4. Setelah selesai fork, maka repository akan masuk ke daftar repo milik Anda.

----
## Time to GO CODE ;)

NB: gunakan `git --help` untuk melihat perintah-perintah git lainnya.

1. Cloning project yang sudah Anda fork ke akun Anda

        git clone <alamat-repo>

    Contoh:

        git clone git@github.com:CreatorB/MaterialTabs.git

2. Untuk mempermudah pengembangan, hendaknya kita menambahkan repository pusat dengan lokal milik kita agar tidak terjadi konflik dengan kontributor lainnya.

        git remote add <nama-repo> <alamat-repo>

    Contoh:

        git remote add upstream git://github.com/neokree/MaterialTabs.git

3. Setelah remote repositori selesai, buatlah branch baru agar tidak merusak history branch utama, dan juga untuk memudahkan racking code.

        git checkout -b <nama-cabang>

    Contoh:

        git checkout -b sample-project

4. Di cabang baru ini lah kita akan untuk melakukan perubahan kode, yang nantinya bisa kita push ke repo pusat. Untuk berpindah branch bisa kita gunakan `git checkout <nama-cabang>`, di mana `<nama-cabang>` adalah nama yang Anda gunakan pada langkah sebelumnya.

5. Setelah melakukan perubahan, kita bisa lakukan commit berisi deskripsi singkat tentang perubahan yang Anda lakukan. Tetapi jika ada penambahan file, bisa menggunakan perintah `git add <nama-file-baru>`, atau gunakan `git add .` untuk menambahkan semua perubahan yang ada di direktori tersebut secara rekursif. Setelah itu baru bisa kita commit.

        git commit -m "<pesan singkat>"

    Contoh:

        git commit -m "fix sample project and added gradle compile"

6. Setelah selesai melakukan commit, kita akan melakukan persiapan untuk membuat *pull request* (biasa disingkat PR) ke repo pusat. Pertama kita pindah branch kembali ke master. 

        git checkout master

7. Setelah itu, kita akan mengambil kode lagi dari pusat, untuk memastikan tidak terdapat konflik pada kontribusi kode kita. Konflik dapat terjadi jika dua atau lebih kontributor melakukan perubahan pada satu berkas, terutama jika perubahan dilakukan pada baris yang sama, terlepas dari apakah tujuan perubahan sama atau tidak.

        git fetch upstream
        git merge upstream/master

8. Dengan proses di atas, setidaknya kita telah bisa memastikan bahwa tidak ada konflik dengan repo pusat. Sekarang kita kembali ke branch lokal development kita `sample-project`.

        git checkout sample-project

9. Setelah itu, kita gabungkan cabang tersebut dengan cabang utama, sehingga kontribusi dapat dikirimkan kembali ke repositori pusat milik neokree, Material Tabs android library, dengan perintah `git rebase <nama-branch>`.

        git rebase master

10. Sebelum push ke repositori pusat milik neokree, kita akan push ke repository hasil fork di awal pembahasan tadi.

        git push origin sample-project

11. Setelah di-push, kita akan melakukan pull request dan membandingkan perubahan yang telah Anda lakukan terhadap repo pusat. Anda juga bisa menyisipkan pesan untuk memberitahukan developer pemilik repo pusat tentang apa yang Anda lakukan. Setelah yakin terhadap perubahan yang telah Anda lakukan, silakan pilih create pull request dan menunggu tanggapan dari pemilik repo pusat. Lebih lengkapnya bisa Anda lihat di tag screenshot.

----
## Screenshot

Ganti branch master ke branch yang Anda buat (sample-project) dan pilih pull request / new pull request.

![branch-sampleproject-creatorbe-screenshot](https://raw.githubusercontent.com/CreatorB/ss/master/git/jasaprogrammer-creatorbe-pondokprogrammer-pull.png)

Setelah itu Anda akan masuk pada konfirmasi pull request, di sini Anda bisa membandingkan code antar branch dan menyisipkan komentar.

![branch-review-creatorbe-screenshot](https://raw.githubusercontent.com/CreatorB/ss/master/git/jasaprogrammer-creatorbe-pondokprogrammer-pull-message.png)

Setelah melakukan pull request maka akan menuju tampilan request yang telah Anda lakukan terhadap project tersebut.

![branch-preview-creatorbe-screenshot](https://raw.githubusercontent.com/CreatorB/ss/master/git/jasaprogrammer-creatorbe-pondokprogrammer-pull-review.png)

----
## Special Thanks

* [bertzzie](https://github.com/bertzzie) - My Friend, who inspired me to write this doc :bowtie:
* [endymuhardin](https://github.com/endymuhardin) - My Friend's Repository Owner, the man whose code inspired (*not only*) me but also many other programmers (*maybe you too*) :wink:
* [creatorbe](https://github.com/creatorb) - Me, yes I'm a man born to code :sunglasses:
