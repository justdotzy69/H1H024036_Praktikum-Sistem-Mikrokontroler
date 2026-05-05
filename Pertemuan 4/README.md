# Laporan Praktikum Sistem Mikrokontroler
## Modul 4: Analog to Digital Converter (ADC) dan Pulse Width Modulation (PWM)

**Nama:** Khoirul Rosyid Gunawan  
**NIM:** H1H024036  

---
## pertanyaan Praktikum 4 Percobaan 1

1. **Apa fungsi perintah analogRead() pada rangkaian praktikum ini?**

Pada rangkaian praktikum ini, analogRead() berfungsi untuk membaca nilai tegangan analog yang dikirimkan oleh potensiometer (sebagai pembagi tegangan) ke pin analog Arduino (misalnya A0).
Perintah ini menggunakan fitur Analog-to-Digital Converter (ADC) di dalam mikrokontroler untuk mengubah tegangan kontinu (biasanya 0–5V) menjadi nilai digital diskrit.
Pada Arduino standar yang memiliki resolusi ADC 10-bit, nilai yang dihasilkan berada dalam rentang 0 hingga 1023


2. **Mengapa diperlukan fungsi map() dalam program tersebut?**

Fungsi map() mutlak diperlukan untuk menyesuaikan (scaling) resolusi data masukan dengan kapasitas data keluaran
Pada program awal (kontrol LED), nilai masukan dari potensiometer memiliki rentang 0–1023 (10-bit), sedangkan fungsi analogWrite() untuk menghasilkan sinyal PWM pada LED hanya menerima nilai dalam rentang 0–255 (8-bit) 
Tanpa map(), pembacaan akan tidak proporsional dan terjadi overflow. Fungsi ini memetakan kedua rentang tersebut secara linear sehingga putaran potensiometer berbanding lurus dengan perubahan kecerahan LED (atau pergerakan sudut motor servo pada kasus lain)


3. **Modifikasi program berikut agar servo hanya bergerak dalam rentang 30° hingga 150°, meskipun potensiometer tetap memiliki rentang ADC 0–1023. Jelaskan program pada file README.md**


```cpp
#include <Arduino.h>
#include <Servo.h> // Library wajib untuk mengontrol motor servo

Servo myServo;           // Membuat objek servo untuk mengontrol motor
const int potPin = A0;   // Pin analog untuk membaca potensiometer
const int servoPin = 9;  // Pin digital PWM yang terhubung ke kabel sinyal servo

int nilaiADC = 0;        // Menyimpan hasil pembacaan potensiometer (0-1023)
int sudutServo = 0;      // Menyimpan hasil konversi sudut (30-150)

void setup() {
  myServo.attach(servoPin); // Menghubungkan pin 9 ke objek myServo

  Serial.begin(9600);       // Mengaktifkan komunikasi serial dengan baud rate 9600
}

void loop() {
  // Baca nilai analog dari potensiometer (rentang 0–1023)
  nilaiADC = analogRead(potPin); 

  // Ubah nilai ADC (0–1023) menjadi batasan sudut servo (30–150 derajat)
  sudutServo = map(nilaiADC, 0, 1023, 30, 150); 

  // Perintahkan motor servo untuk bergerak ke sudut yang telah dipetakan
  myServo.write(sudutServo); 

  // Tampilkan data ADC dan Sudut ke Serial Monitor
  Serial.print("ADC: ");
  Serial.print(nilaiADC);
  Serial.print(" | Sudut Servo: ");
  Serial.print(sudutServo);
  Serial.println("°");

  // Delay untuk memberi waktu servo mencapai posisi dan menstabilkan serial
  delay(50); 
}
```
