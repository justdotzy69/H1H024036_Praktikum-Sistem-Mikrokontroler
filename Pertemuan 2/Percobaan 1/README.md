Jawaban pertanyaan di percobaan 1 Praktikum Sistem Mikrokontroler di Modul 2 Pemrograman GPIO

Nama : Khoirul Rosyid Gunawan

NIM : H1H024036

### 2.5.4 Pertanyaan Praktikum : 

1. Gambarkan rangkaian schematic yang digunakan pada percobaan!

2. Apa yang terjadi jika nilai num lebih dari 15?

3. Apakah program ini menggunakan common cathode atau common anode? Jelaskan
alasanya!

4. Modifikasi program agar tampilan berjalan dari F ke 0 dan berikan penjelasan disetiap
baris kode nya dalam bentuk README.md!


jawaban 

1.<img width="989" height="765" alt="image" src="https://github.com/user-attachments/assets/6e22aa04-1a68-4cba-9b64-b66992d09a33" />

2. Apabila nilai num melebihi 15, program akan mengakses indeks array digitPattern di luar rentang yang telah didefinisikan (indeks 0–15). Dalam bahasa C++ pada Arduino, akses out-of-bounds tidak memicu error kompilasi melainkan membaca data memori arbitrer di sekitar array, menghasilkan tampilan acak pada segmen. Untuk mencegah hal ini, perlu ditambahkan validasi: if (num < 0 || num > 15) return; di awal fungsi displayDigit()

3. Program ini menggunakan tipe common cathode (CC). Hal ini terbukti dari pola bit pada array digitPattern: digit '0' direpresentasikan sebagai {1,1,1,1,1,1,0,0}, di mana nilai 1 berarti logika HIGH (menyalakan segmen). Pada common cathode, pin katoda bersama terhubung ke GND sehingga tegangan HIGH pada anoda segmen akan mengalirkan arus dan menyalakannya. Sebaliknya, pada common anode dibutuhkan logika LOW untuk menyalakan segmen

4.   ```cpp
     void loop() {
       // Mulai dari 15 (=F hex), turun ke 0
       for(int i=15; i>=0; i--) {
         displayDigit(i); // tampilkan digit
         delay(1000);     // jeda 1 detik
          }
        }
      }


