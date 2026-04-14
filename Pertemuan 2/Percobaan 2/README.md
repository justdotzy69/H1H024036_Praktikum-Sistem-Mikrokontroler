Jawaban pertanyaan di percobaan 1 Praktikum Sistem Mikrokontroler di Modul 2 Pemrograman GPIO

Nama : Khoirul Rosyid Gunawan

NIM : H1H024036

### 2.6.4 Pertanyaan Praktikum :

1. Gambarkan rangkaian schematic yang digunakan pada percobaan!
   
2. Mengapa pada push button digunakan mode INPUT_PULLUP pada Arduino Uno?
Apa keuntungannya dibandingkan rangkaian biasa?

3. Jika salah satu LED segmen tidak menyala, apa saja kemungkinan penyebabnya dari
sisi hardware maupun software?

4. Modifikasi rangkaian dan program dengan dua push button yang berfungsi sebagai
penambahan (increment) dan pengurangan (decrement) pada sistem counter dan
berikan penjelasan disetiap baris kode nya dalam bentuk README.md!

Jawaban

1. <img width="1045" height="782" alt="image" src="https://github.com/user-attachments/assets/18b55af5-f07e-413c-a154-bf0b808e6e4f" />

2. Mode INPUT_PULLUP mengaktifkan resistor pull-up internal ATMega328 (~20–50 kΩ) untuk menghindari kondisi pin mengambang (floating) yang dapat menyebabkan pembacaan nilai acak akibat gangguan elektromagnetik [1]. Keunggulannya dibanding rangkaian biasa: (1) menghemat komponen—tidak perlu resistor pull-up eksternal; (2) sinyal lebih stabil dengan referensi tegangan HIGH yang jelas; (3) desain rangkaian lebih sederhana dan hemat ruang pada breadboard.

3. Dari sisi hardware: (1) koneksi kabel jumper longgar atau putus pada jalur segmen; (2) resistor 220 Ω putus atau nilai tidak sesuai; (3) segmen LED pada display rusak; (4) pin Arduino yang bersangkutan mengalami kerusakan. Dari sisi software: (1) pola bit pada array digitPattern salah untuk segmen tersebut; (2) nomor pin pada array segmentPins tidak sesuai sambungan fisik; (3) fungsi pinMode() belum dipanggil untuk pin tersebut [3].

4. ```cpp
   const int btnUp=2, btnDown=3;
    bool lastUp=HIGH, lastDown=HIGH;
    void loop() {
      bool sUp=digitalRead(btnUp);
      bool sDn=digitalRead(btnDown);
    // Tepi turun Up = increment
    if(lastUp==HIGH && sUp==LOW){
      if(++currentDigit>15) currentDigit=0;
      displayDigit(currentDigit);
    }
    // Tepi turun Down = decrement
    if(lastDown==HIGH && sDn==LOW){
      if(--currentDigit<0) currentDigit=15;
      displayDigit(currentDigit);
    }

