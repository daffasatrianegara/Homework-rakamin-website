# HOMEWORK JS : WEBSITE PENGHITUNG RATA-RATA
## Soal Homework

Buatlah 1 buah halaman yang berisi form dan tabel. Form dan tabel terpisah menjadi 2 buah tab dengan nama tab Registrasi dan tab List Pendaftar. Form memiliki isian:
- Nama minimal 10 karakter.
- Umur minimal 25 tahun.
- Uang sangu minimal 100 ribu dan maksimal 1 juta.



Setiap melakukan submit akan dicek berdasarkan kriteria tersebut, data tidak akan bisa tersimpan jika kriteria masih salah. Pada tab List pendaftar akan berisi tabel yang berisi pendaftar dan resume dibawahnya, isi resume seperti berikut:

Rata rata pendaftar memiliki uang sangu sebesar [nilai rata rata uang sangu] dengan rata rata umur [nilai rata rata umur].

Catatan:
- Imajinasikan dan coba terapkan salah satu konsep OOP dan Asynchronous dalam menyelesaikan masalah tersebut.
- Push ke REPO dan upload link REPO, tolong repo dalam status public.
- Gunakan framework CSS untuk membuat tampilan lebih menarik.



# penjelasan kode Javascript 
## class prosesData
Class `prosesData` merupakan sebuah kelas JavaScript yang digunakan untuk mengelola data dan validasi pada halaman web yang memiliki form dan tabel. Kelas ini memiliki beberapa metode dan properti yang memungkinkan pengguna untuk memasukkan data, memvalidasi data, dan menghitung rata-rata dari data yang dimasukkan.



Berikut source code dari `prosesData` :
```javascript
      class prosesData {
        constructor() {
          this.data = [];
          this.submitButton = document.getElementById("submit");
          this.namaInput = document.getElementById("nama");
          this.umurInput = document.getElementById("umur");
          this.uangskInput = document.getElementById("uangsk");
          this.ceknama = document.getElementById("ceknama");
          this.cekumur = document.getElementById("cekumur");
          this.cekuang = document.getElementById("cekuang");

          this.submitButton.disabled = true;

          this.namaInput.addEventListener(
            "input",
            this.validasinama.bind(this)
          );
          this.umurInput.addEventListener(
            "input",
            this.validasiumur.bind(this)
          );
          this.uangskInput.addEventListener(
            "input",
            this.validasiuang.bind(this)
          );
          this.submitButton.addEventListener(
            "click",
            this.submitData.bind(this)
          );
        }

        validasinama() {
          if (this.namaInput.value === "") {
            this.ceknama.innerHTML = "Data masih kosong.";
            this.ceknama.style.color = "red";
          } else if (this.namaInput.value.length < 10) {
            this.ceknama.innerHTML =
              "Mohon masukkan nama Anda dengan panjang minimal 10 karakter.";
            this.ceknama.style.color = "red";
          } else {
            this.ceknama.innerHTML = "Panjang karakter sesuai.";
            this.ceknama.style.color = "green";
          }
          this.cek();
        }

        validasiumur() {
          if (this.umurInput.value === "") {
            this.cekumur.innerHTML = "Data masih kosong.";
            this.cekumur.style.color = "red";
          } else if (this.umurInput.value < 25) {
            this.cekumur.innerHTML =
              "Silahkan masukkan usia Anda dengan syarat minimal 25 tahun.";
            this.cekumur.style.color = "red";
          } else {
            this.cekumur.innerHTML = "Umur sesuai.";
            this.cekumur.style.color = "green";
          }
          this.cek();
        }

        validasiuang() {
          if (this.uangskInput.value === "") {
            this.cekuang.innerHTML = "Data masih kosong.";
            this.cekuang.style.color = "red";
          } else if (this.uangskInput.value < 100000) {
            this.cekuang.innerHTML =
              "Silahkan masukkan uang saku dengan minimal 100.000.";
            this.cekuang.style.color = "red";
          } else if (this.uangskInput.value > 1000000) {
            this.cekuang.innerHTML =
              "Batas maksimal uang saku adalah 1.000.000.";
            this.cekuang.style.color = "red";
          } else {
            this.cekuang.innerHTML = "Uang saku sesuai.";
            this.cekuang.style.color = "green";
          }
          this.cek();
        }

        cek() {
          if (
            this.ceknama.style.color === "green" &&
            this.cekumur.style.color === "green" &&
            this.cekuang.style.color === "green"
          ) {
            this.submitButton.disabled = false;
          } else {
            this.submitButton.disabled = true;
          }
        }

        submitData() {
          let getnama = this.namaInput.value;
          let getumur = this.umurInput.value;
          let getuang = this.uangskInput.value;

          let push = {
            nama: getnama,
            umur: getumur,
            uangsk: getuang,
          };

          this.data.push(push);
          console.log(this.data);

          this.namaInput.value = null;
          this.umurInput.value = null;
          this.uangskInput.value = null;

          this.ceknama.innerHTML = "";
          this.ceknama.style.color = "red";
          this.cekumur.innerHTML = "";
          this.cekumur.style.color = "red";
          this.cekuang.innerHTML = "";
          this.cekuang.style.color = "red";
          this.cek();

          let tabeldata = document.getElementById("datatabel");
          const node = document.createElement("tr");
          const nodenama = document.createElement("td");
          const nodeumur = document.createElement("td");
          const nodeuangsk = document.createElement("td");
          const nilainm = document.createTextNode(getnama);
          const nilaiumr = document.createTextNode(getumur);
          const nilaiuang = document.createTextNode(getuang);

          nodenama.appendChild(nilainm);
          nodeumur.appendChild(nilaiumr);
          nodeuangsk.appendChild(nilaiuang);

          node.appendChild(nodenama);
          node.appendChild(nodeumur);
          node.appendChild(nodeuangsk);

          setTimeout(() => {
            tabeldata.appendChild(node);
          }, 3000);
          setTimeout(() => {
            this.rerata();
          }, 4000);
        }

        rerata() {
          let rerataumur = 0;
          let reratauang = 0;
          this.data.forEach((item) => {
            let sangu = item.uangsk;
            let usia = item.umur;

            sangu = parseFloat(sangu);
            usia = parseFloat(usia);

            rerataumur += usia;
            reratauang += sangu;
          });

          rerataumur /= this.data.length;
          reratauang /= this.data.length;

          const hasilrerata = document.getElementById("hasil");
          hasilrerata.innerHTML = `Rata rata pendaftar memiliki uang saku sebesar ${reratauang} dengan rata rata umur ${rerataumur}`;
        }
      }
      const proses = new prosesData();
```
Berikut penjelasan lebih detail tentang kelas ini:

## Properti:

- `data`: Sebuah array yang digunakan untuk menyimpan data pendaftar yang telah dimasukkan melalui form.

- `submitButton`, `namaInput`, `umurInput`, `uangskInput`, `ceknama`, `cekumur`, `cekuang`: Properti ini digunakan untuk mengakses elemen-elemen HTML dalam halaman web seperti tombol submit, input nama, input umur, input uang saku, dan elemen-elemen untuk menampilkan pesan validasi.

## Metode:

- `constructor()`: Ini adalah constructor yang dijalankan saat objek dari kelas `prosesData` dibuat. Pada constructor, elemen-elemen HTML diambil, event listener ditambahkan, dan tombol submit dinonaktifkan awalnya.

- `validasinama()`, `validasiumur()`, `validasiuang()`: Metode-metode ini digunakan untuk melakukan validasi pada input nama, umur, dan uang saku sesuai dengan kriteria yang telah ditentukan. Mereka akan menampilkan pesan validasi yang sesuai dan memanggil metode `cek()` untuk memeriksa apakah semua validasi telah memenuhi syarat.

- `cek()`: Metode ini memeriksa apakah semua pesan validasi memiliki warna hijau (valid) atau tidak. Jika semua validasi telah memenuhi syarat, tombol submit akan diaktifkan.

- `submitData()`: Metode ini dijalankan saat tombol submit ditekan. Data yang dimasukkan akan disimpan dalam array `data`, pesan validasi akan dihapus, dan elemen-elemen input akan dikosongkan. Selain itu, data pendaftar juga akan ditambahkan ke dalam tabel setelah beberapa detik dan metode `rerata()` akan dipanggil untuk menghitung rata-rata umur dan uang saku dari data pendaftar.

- `rerata()`: Metode ini digunakan untuk menghitung rata-rata umur dan uang saku dari data pendaftar yang telah dimasukkan. Hasil rata-rata ini akan ditampilkan dalam elemen HTML.

Kelas `prosesData` digunakan untuk menciptakan interaksi antara pengguna dan halaman web untuk mengelola data pendaftar dengan syarat dan perhitungan yang telah ditentukan.

# Penggunaan Promise dalam Penanganan Tindakan Asinkron

Dalam proyek ini, kita menggunakan Promise dalam JavaScript untuk menangani tindakan asinkron secara berurutan, khususnya untuk menampilkan pesan sukses setelah tombol "Submit" ditekan pada halaman web. 


Berikut source code untuk metode promise :
```javascript

      let pernama = document.getElementById("pernama");
      let perumur = document.getElementById("perumur");
      let peruang = document.getElementById("peruang");
      let button = document.getElementById("submit");
      const input = (info) => {
        return new Promise((resolve, reject) => {
          resolve("&#8226;"+ " Data " + info + " berhasil terinput");
        });
      };

      let nama = "nama";
      let umur = "umur";
      let uang = "uang";
      Promise.all([input(nama), input(umur), input(uang)]).then((result) => {
        button.addEventListener("click", function () {
          setTimeout(() => {
            pernama.innerHTML = "<span style='color: green;'>" + result[0] + "</span>";           
          }, 1000);
          setTimeout(() => {
            perumur.innerHTML = "<span style='color: green;'>" + result[1] + "</span>";           
          }, 2000);
          setTimeout(() => {
            peruang.innerHTML = "<span style='color: green;'>" + result[2] + "</span>";           
          }, 3000);
          setTimeout(() => {
            pernama.innerHTML = "";
            perumur.innerHTML = "";
            peruang.innerHTML = "";
          }, 6000);
        });
      });
```
Berikut adalah penjelasan singkat tentang bagaimana kode ini bekerja:

## Penerapan Promise:

1. Pertama, kita mendefinisikan elemen-elemen HTML yang akan digunakan untuk menampilkan pesan sukses. Ini mencakup elemen `pernama` untuk pesan sukses nama, `perumur` untuk pesan sukses umur, dan `peruang` untuk pesan sukses uang.

2. Kita juga mendapatkan referensi ke elemen tombol "Submit" dengan `button`.

3. Kemudian, kita mendefinisikan sebuah fungsi bernama `input(info)` yang mengembalikan sebuah Promise. Fungsi ini menerima informasi `info` sebagai argumen dan kemudian mengembalikan pesan sukses dengan informasi tersebut.

4. Selanjutnya, kita menginisialisasi variabel `nama`, `umur`, dan `uang` dengan string yang mewakili jenis informasi yang akan ditampilkan dalam pesan sukses.

5. Menggunakan `Promise.all()`, kita memanggil fungsi `input(nama)`, `input(umur)`, dan `input(uang)` secara bersamaan. Ini akan memicu tiga Promise yang akan menampilkan pesan sukses secara bersamaan setelah tombol "Submit" ditekan.

6. Dalam `.then()`, kita menambahkan event listener pada tombol "Submit". Ketika tombol tersebut ditekan, kita menggunakan `setTimeout` untuk menampilkan pesan sukses satu per satu dengan interval tertentu (dalam milidetik).

7. Setelah masing-masing pesan sukses ditampilkan, kita menggunakan `setTimeout` lagi untuk menghapus pesan-pesan tersebut setelah beberapa detik.

## Hasil:

Kode ini menghasilkan pengalaman yang lebih responsif bagi pengguna dengan menampilkan pesan sukses dalam urutan yang terorganisir dan dengan tampilan yang menyenangkan setelah mereka mengisi formulir dan menekan tombol "Submit".
