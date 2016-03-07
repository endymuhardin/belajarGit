# Berkontribusi di Proyek git

----
## What
see [Pondokprogrammer](pondokprogrammer.com/tutorial-github/)

> Mungkin sudah terlalu sering kita bekerja mandiri pada project pribadi, disini saya akan menunjukkan bagaimana caranya berkontribusi di project lain. Hal termudah untuk mencapai itu kita cari proyek open sources, apa itu proyek opensources, Proyek perangkat lunak open source merupakan proyek yang memberikan kode program kepada penggunanya secara bebas, dan tak jarang pengembangannya dilakukan secara terbuka: siapapun boleh berkontribusi dalam menulis kode tersebut. Menggunakan perangkat lunak open source tentunya sangat baik, karena selain tidak melakukan pembajakan, kita juga mendukung para pengembang dari perangkat lunak yang kita gunakan. Tetapi, akan lebih baik lagi jika kita juga ikut berkontribusi, mulai dari kontribusi pengunaan, pelaporan bug, sampai dengan kontribusi kode. Kontribusi pada proyek open source akan membantu kita untuk meningkatkan kemampuan pengembangan perangkat lunak.".

----
## Bagaimana

**Welcome Contributors**

* Fork it!
* Create your feature branch: `git checkout -b my-new-feature`
* Commit your changes: `git commit -am 'Add some feature'`
* Push to the branch: `git push origin my-new-feature`
* Submit a pull request ;)

Tahapan berkontribusi yang baik,

1. Cari proyek opensources. *(disini saya sebagai pengembang android saya akan memberikan contoh cara berkontribusi di salah satu proyek opensources pada library android)*

  **[Material Tabs](https://github.com/neokree/MaterialTabs)**

2. Coba cari cari info tentang aturan kontribusi atau bisa dengan cara melakukan pendekatan pada developer terkait baik via email atau sosial akun.

3. Jika memang telah melakukan pendekatan atau tidak ada jalur lain untuk menempuh jalan tersebut, anda bisa langsung saja untuk melakukan fork project yang akan berkontribusi di dalamnya dimana nantinya kode akan di review oleh pemilik repository untuk di gabungkan atau tidaknya.

4. Setelah selesai fork, maka repository akan masuk ke list repo milik anda, untuk selanjutnya mungkin langsung saja, kali ini masuk pada git command apa aja yang bisa kita gunakan untuk berkontribusi di proyek opensources beserta simulasi yang akan saya contoh kan untuk berkontribusi pada proyek library android - material tabs  .

----
## Time to GO CODE ;)
### ` git --help` to see another commands


>cloning project yang suda anda fork ke akun anda 

    git clone git@github.com:CreatorB/MaterialTabs.git

>untuk memudahkan development, hendaknya kita menambahkan repository pusat dengan lokal milik kita agar tidak terjadi konflik dengan kontributor lainnya. `git remote add <nama-repo> <alamat-repo>`

    git remote add upstream git://github.com/neokree/MaterialTabs.git

>Setelah remote repositori selesai dan seperti yang saya katakan tadi agar kita tidak hanya asal berkontribusi tapi memang benar benar clear dalam membantu development maka kita hendaknya juga membuat branch baru terlebih dahulu agar tidak merusak history dan nantinya juga akan memudahkan untuk racking code. `git checkout -b <nama-cabang>`

    git checkout -b sample-project

>Okkay sekarang setelah remote dan cabang / branch dibuat maka di cabang baru ini lah kita akan untuk melakukan perubahan kode yang nantinya bisa kita push ke repo pusat. Untuk berpindah branch bisa kita gunakan . `git checkout <nama-cabang>` dimana di repo lokal saya sekarang ada dua cabang cabang pertama adalah master dan cabang kedua adalah sample-project dan di sample-project saya bisa melakukan banyak perubahan kode. Jika memang ada penambahan file bisa menggunakan `git add <nama-file-baru>` atau kalo memang banyak file yang ditambahkan dan males add satu per satu, anda bisa langsung menuju root dari repository lalu `git add .` ya dot / titik berarti menambahkan semua perubahan yang ada di direktori tersebut secara rekursif. Setalah selesai perubahan silahkan lakukan commit, berisi pesan apa yang telah anda kontribusikan / lakukan perubahan.

    git commit -m "fixing sample project and compile gradle added"

>Setelah selesai melakukan commit, kita lakukan persiapan untuk pull ke repo pusat, sebelum itu kita pindah branch dulu ke master. 

    git checkout master

>Setelah itu, kita akan mengambil kode lagi dari pusat, untuk memastikan tidak terdapat konflik pada kontribusi kode kita. Konflik dapat terjadi jika dua atau lebih kontributor melakukan perubahan pada satu berkas, terutama jika perubahan dilakukan pada baris yang sama, terlepas dari apakah tujuan perubahan sama atau tidak. Perintah yang digunakan adalah `git fetch` dan `git merge` pada cabang utama.

    git fetch upstream

>Setelah fetching selesai, sekarang kita merge dengan master.

    git merge upstream/master

>Dengan beberapa proses diatas, setidaknya kita telah bisa memastikan bahwanya tidak ada konflik dengan repo pusat. Sekarang kita kembali ke branch lokal development saya .  `sample-project` 

    git checkout sample-project

>dan menggabungkan cabang tersebut dengan cabang utama, sehingga kontribusi dapat dikirimkan kembali ke repositori pusat milik neokree, Material Tabs android library, dengan perintah `git rebase <nama-branch>`

    git rebase master

>Sebelum push ke repositori pusat milik neokree, maka terlebih dahulu akan saya push ke repository milik saya hasil fork di awal pembahasan tadi, dimana banyak perubahan yang dilakukan di branch tersebut.

    git push origin sample-project

>Setelah di push maka sekarang kita pergi menuju browser untuk melakukan pull request, cek / compare perubahan apa yang telah anda lakukan di branch anda pada branch master milik repo pusat, dan anda juga bisa menyisipkan pesan untuk memberitahukan developer pemilik repo pusat tentang apa yang anda lakukan, setelah yakin terhadap perubahan yang telah anda lakukan silahkan pilih create pull request dan selamat menunggu pemilik repo pusat untuk menanggapi di terima tidaknya kontribusi anda. Lebih lengkapnya bisa anda lihat di tag screenshot

----
## Screenshot

Ganti branch master ke branch yang anda buat (sample-project) dan pilih pull request / new pull request.

![branch-sampleproject-creatorbe-screenshot](https://raw.githubusercontent.com/CreatorB/ss/master/git/jasaprogrammer-creatorbe-pondokprogrammer-pull.png)

Setelah itu anda akan masuk pada konfirmasi pull request, disini anda bisa membandingkan code antar branch dan menyisipkan komentar.

![branch-review-creatorbe-screenshot](https://raw.githubusercontent.com/CreatorB/ss/master/git/jasaprogrammer-creatorbe-pondokprogrammer-pull-message.png)

Setelah melakukan pull request maka akan menuju tampilan request yang telah anda lakukan terhadap project tersebut.

![branch-preview-creatorbe-screenshot](https://raw.githubusercontent.com/CreatorB/ss/master/git/jasaprogrammer-creatorbe-pondokprogrammer-pull-review.png)

----
## Special Thank's

* [bertzzie](https://github.com/bertzzie) - My Friend's, who inspire me to write this doc :bowtie:
* [endymuhardin](https://github.com/endymuhardin) - My Friend's Repository Owner, man who code inspire (*not only*) me but also many programmers (*maybe too*) :wink:
* [creatorbe](https://github.com/creatorb) - me, yes i'm a man who born to code :sunglasses:
