Jawaban pertanyaan di percobaan 2 Praktikum Sistem Mikrokontroler di Modul 1 Percabangan dan Perulangan

Nama : Khoirul Rosyid Gunawan

NIM : H1H024036

### 1.6.4 Pertanyaan Praktikum : 
1.  Rangkaian Schematic 5 LED Running


  <img width="988" height="761" alt="image" src="https://github.com/user-attachments/assets/c81f055a-3565-46d9-b372-0be1fcf076cb" />


2. Jelaskan bagaimana program membuat efek LED berjalan dari kiri ke kanan!
   

Menggunakan perulangan for yang terus menambah nilai pin target (ledPin++) mulai dari Pin 2 perlahan naik hingga Pin 7, lalu menyalakan dan mematikannya bergiliran.


3. Jelaskan bagaimana program membuat LED kembali dari kanan ke kiri!


4. Buatkan program agar LED menyala tiga LED kanan dan tiga LED kiri secara bergantian dan berikan penjelasan disetiap baris kode nya dalam bentuk README.md!


Berikut adalah source code beserta penjelasan di setiap barisnya:


Menggunakan perulangan for yang terus mengurangi nilai pin target (ledPin--) mulai dari Pin 7 perlahan turun mundur hingga Pin 2.

 ```cpp 
int timer = 500;                // Set jeda pergantian 500ms

void setup() {                  // Fungsi inisialisasi awal
  for (int ledPin = 2; ledPin <= 7; ledPin++) { // Looping dari pin 2 sampai 7
    pinMode(ledPin, OUTPUT);    // Atur pin tersebut sebagai output
  }                             // Tutup blok for inisialisasi
}                               // Tutup blok setup

void loop() {                   // Fungsi utama berulang

  // --- KONDISI 1: 3 KIRI NYALA, 3 KANAN MATI ---
  for (int pinKiri = 2; pinKiri <= 4; pinKiri++) { // Looping pin kiri (2,3,4)
    digitalWrite(pinKiri, HIGH);// Nyalakan LED kiri
  }                             // Tutup blok for kiri nyala
  for (int pinKanan = 5; pinKanan <= 7; pinKanan++) { // Looping pin kanan (5,6,7)
    digitalWrite(pinKanan, LOW);// Matikan LED kanan
  }                             // Tutup blok for kanan mati
  delay(timer);                 // Tahan kondisi ini selama 500ms

  // --- KONDISI 2: 3 KANAN NYALA, 3 KIRI MATI ---
  for (int pinKanan = 5; pinKanan <= 7; pinKanan++) { // Looping pin kanan (5,6,7)
    digitalWrite(pinKanan, HIGH);// Nyalakan LED kanan
  }                             // Tutup blok for kanan nyala
  for (int pinKiri = 2; pinKiri <= 4; pinKiri++) { // Looping pin kiri (2,3,4)
    digitalWrite(pinKiri, LOW); // Matikan LED kiri
  }                             // Tutup blok for kiri mati
  delay(timer);                 // Tahan kondisi ini selama 500ms
  
}                               // Tutup blok loop
