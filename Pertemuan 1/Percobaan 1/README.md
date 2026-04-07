Jawaban pertanyaan di percobaan 1 Praktikum Sistem Mikrokontroler di Modul 1 Percabangan dan Perulangan

Nama : Khoirul Rosyid Gunawan
NIM : H1H024036

1.5.4 Pertanyaan Praktikum : 
1. Pada kondisi apa program masuk ke blok if?
Program akan masuk dan memproses pernyataan di dalam blok if ketika kondisi variabel timeDelay bernilai kurang dari atau sama dengan 100 (yaitu timeDelay <= 100)
2. Pada kondisi apa program masuk ke blok else?
Program masuk ke blok else ketika kondisi pada instruksi if tidak terpenuhi atau tidak sesuai [cite: 36]. Dalam konteks program ini, artinya blok else akan dieksekusi ketika nilai timeDelay masih lebih besar dari 100. Jika kondisi tersebut terpenuhi, program akan mengeksekusi instruksi di dalamnya, yaitu mengurangi nilai timeDelay sebesar 100
3. Apa fungsi dari perintah delay(timeDelay)?
Fungsi dari perintah delay(timeDelay) adalah untuk memberikan jeda waktu (dalam milidetik) sebesar nilai yang tersimpan dalam variabel timeDelay [cite: 87, 90]. Jeda ini menentukan durasi seberapa lama LED akan menyala (HIGH) atau mati (LOW) sebelum melanjutkan ke baris kode berikutnya
Jika program yang dibuat memiliki alur mati → lambat → cepat → reset (mati), ubah menjadi LED tidak langsung reset → tetapi berubah dari cepat → sedang → mati dan berikan penjelasan disetiap baris kode nya dalam bentuk README.md!

## 1. Jawaban 1.5.4 (Percabangan Kustom)
**Tujuan:** Mengubah alur program agar LED tidak langsung *reset*, melainkan berubah dari kedipan makin cepat &rarr; sedang &rarr; mati.

Berikut adalah source code beserta penjelasan di setiap barisnya:

```cpp
const int ledPin = 6;     // Mendeklarasikan variabel konstan untuk LED yang terhubung di Pin 6
int timeDelay = 1000;     // Menetapkan nilai awal jeda waktu (delay) sebesar 1000ms (1 detik)
int phase = 0;            // Variabel untuk melacak fase: 0 (Makin Cepat), 1 (Sedang), 2 (Mati)

void setup() {            // Fungsi bawaan yang hanya dieksekusi satu kali saat Arduino dinyalakan
  pinMode(ledPin, OUTPUT);// Mengonfigurasi Pin 6 sebagai OUTPUT untuk mengeluarkan tegangan listrik
}                         // Menutup blok fungsi setup()

void loop() {             // Fungsi bawaan yang akan dieksekusi secara berulang-ulang tanpa henti
  
  // --- FASE 0: KEDIPAN MAKIN CEPAT ---
  if (phase == 0) {             // Mengecek apakah program saat ini sedang berada di fase 0
    digitalWrite(ledPin, HIGH); // Menyalakan LED (memberi tegangan)
    delay(timeDelay);           // Memberi jeda nyala selama nilai timeDelay saat ini
    digitalWrite(ledPin, LOW);  // Mematikan LED (memutus tegangan)
    delay(timeDelay);           // Memberi jeda mati selama nilai timeDelay saat ini
    
    timeDelay -= 100;           // Mengurangi nilai timeDelay sebanyak 100ms agar kedipan makin cepat
    
    if (timeDelay <= 100) {     // Percabangan baru: mengecek jika jeda sudah <= 100ms (sangat cepat)
      phase = 1;                // Jika ya, ubah variabel phase menjadi 1 untuk berpindah fase
      timeDelay = 500;          // Set ulang nilai timeDelay menjadi 500ms untuk kecepatan "sedang"
    }                           // Menutup blok if (timeDelay <= 100)
  }                             // Menutup blok if (phase == 0)
  
  // --- FASE 1: KEDIPAN SEDANG KONSTAN ---
  else if (phase == 1) {        // Jika tidak di fase 0, cek apakah sedang di fase 1
    for(int i = 0; i < 5; i++) {// Perulangan for untuk membuat LED berkedip konstan sebanyak 5 kali
      digitalWrite(ledPin, HIGH); // Menyalakan LED
      delay(timeDelay);           // Menahan nyala selama 500ms (nilai timeDelay di fase 1)
      digitalWrite(ledPin, LOW);  // Mematikan LED
      delay(timeDelay);           // Menahan mati selama 500ms
    }                             // Menutup blok perulangan for
    phase = 2;                  // Setelah 5 kali berkedip, ubah phase ke 2 untuk berpindah fase
  }                             // Menutup blok else if (phase == 1)
  
  // --- FASE 2: MATI / ISTIRAHAT ---
  else if (phase == 2) {        // Jika tidak di fase 0 dan 1, cek apakah sedang di fase 2
    digitalWrite(ledPin, LOW);  // Memastikan LED dimatikan
    delay(3000);                // Menahan kondisi mati/istirahat ini selama 3000ms (3 detik)
    
    phase = 0;                  // Mereset variabel phase kembali ke 0 agar siklus mengulang dari awal
    timeDelay = 1000;           // Mereset timeDelay kembali ke 1000ms seperti saat pertama kali
  }                             // Menutup blok else if (phase == 2)
}                               // Menutup blok fungsi loop()
