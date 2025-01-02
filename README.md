# arduino-sensor

 Skema Sirkuit Project Arduino **SENSOR ULTRASONIC**




## **Komponen" Yang Dibutuhkan (Tools & Materials):**

###### • 1).Arduino Uno:
Sebagai mikrokontroler utama untuk memproses data dari sensor dan mengontrol aktuator.
###### • 2).Sensor Ultrasonik HC-SR04:
Digunakan untuk mendeteksi jarak objek dengan prinsip pantulan gelombang suara.
###### • 3).Motor Servo (SG90):
Digunakan untuk menggerakkan komponen mekanis, seperti membuka atau menutup.
###### • 4).Breadboard:
Media untuk koneksi sementara antar komponen elektronik.
###### • 5).Kabel Jumper:
Menghubungkan komponen dengan mikrokontroler atau breadboard.
###### • 6).Sumber Daya:
Arduino dihubungkan dengan sumber daya melalui kabel USB.


## Background Project:
  Proyek ini bertujuan untuk membuat sistem berbasis Arduino menggunakan sensor ultrasonik dan servo yang dapat digunakan dalam berbagai aplikasi. Beberapa latar belakang proyek ini meliputi:
### • **Otomasi sederhana:**
Untuk membantu proses otomatisasi, seperti tempat sampah otomatis, penghalang otomatis, atau sistem pengukur jarak.

### • **Kemudahan pengguna:** 
Mengurangi interaksi langsung manusia dengan perangkat mekanis atau meningkatkan kenyamanan dengan sistem berbasis jarak.

### • **Penerapan edukatif**
 Proyek ini cocok sebagai pengantar pemrograman Arduino dan pemahaman tentang sensor dan aktuator.

## Penjelasan Project:

 #### **Fungsi Utama:**
Proyek ini memanfaatkan sensor ultrasonik HC-SR04 untuk mendeteksi keberadaan objek di depannya. Jika objek terdeteksi dalam jarak tertentu, motor servo bergerak (misalnya membuka tutup, menggerakkan penghalang, dll.).

 #### **Cara Kerja:**

    Sensor Ultrasonik mengirimkan gelombang suara ultrasonik dan menunggu pantulan kembali.
    Arduino menghitung waktu yang dibutuhkan gelombang untuk kembali, mengonversinya menjadi jarak.
    Jika jarak yang terdeteksi berada di bawah ambang tertentu (misalnya, 10 cm), Arduino mengirimkan sinyal ke motor servo.
    Motor Servo kemudian bergerak sesuai logika pemrograman (misalnya membuka 90 derajat).
    Setelah waktu tertentu, servo kembali ke posisi semula.
    Penerapan Proyek:

     • Tempat sampah otomatis.
     • Sistem penghalang pintu otomatis.
     • Proyek edukasi tentang sensor jarak dan aktuator mekanis.


## Skema Gambar Sirkuit:

![Neat](https://github.com/user-attachments/assets/acf723ee-8832-4bd9-afd1-d13451949d71)



## Sketch Code Program:

```cpp
const int pinSensor = 3;
const int merah = 4;
const int kuning = 6;
const int oranye = 5;
const int buzzer = 7;
const int biru = 11;
const int putih = 8; 

void setup() {
  // Inisialisasi komunikasi serial:
  Serial.begin(9600);
  pinMode(merah, OUTPUT);
  pinMode(kuning, OUTPUT);
  pinMode(oranye, OUTPUT);
  pinMode(buzzer, OUTPUT);
  pinMode(putih, OUTPUT);
  pinMode(biru, OUTPUT);
}

void loop() {
  // Variabel untuk durasi ping dan jarak dalam cm:
  long durasi, cm;

  // Memicu sensor ultrasonik dengan pulsa HIGH selama 2 mikrodetik atau lebih.
  // Berikan pulsa LOW singkat terlebih dahulu untuk memastikan sinyal bersih:
  pinMode(pinSensor, OUTPUT);
  digitalWrite(pinSensor, LOW);
  delayMicroseconds(100);
  digitalWrite(pinSensor, HIGH);
  delayMicroseconds(1000);
  digitalWrite(pinSensor, LOW);

  // Pin yang sama digunakan untuk membaca sinyal pantulan dari sensor:
  pinMode(pinSensor, INPUT);
  durasi = pulseIn(pinSensor, HIGH);

  // Konversi waktu ke jarak dalam cm:
  cm = mikrodetikKeCentimeter(durasi);

  // Cetak jarak yang terdeteksi:
  Serial.print("Jarak: ");
  Serial.print(cm);
  Serial.println(" cm");

  // Logika untuk menyalakan LED atau buzzer berdasarkan jarak objek:
  if (cm > 150) {
    digitalWrite(merah, LOW);
    digitalWrite(kuning, LOW);
    digitalWrite(oranye, LOW);
    digitalWrite(buzzer, LOW);
    digitalWrite(biru, HIGH);
    digitalWrite(putih, LOW);
  } else if (cm > 130 && cm <= 150) {
    digitalWrite(merah, LOW);
    digitalWrite(kuning, LOW);
    digitalWrite(oranye, LOW);
    digitalWrite(buzzer, LOW);
    digitalWrite(biru, LOW);
    digitalWrite(putih, HIGH);
  } else if (cm > 100 && cm <= 130) {
    digitalWrite(merah, LOW);
    digitalWrite(kuning, HIGH);
    digitalWrite(oranye, LOW);
    digitalWrite(buzzer, LOW);
    digitalWrite(biru, LOW);
    digitalWrite(putih, LOW);
  } else if (cm > 80 && cm <= 100) {
    digitalWrite(merah, LOW);
    digitalWrite(kuning, LOW);
    digitalWrite(oranye, HIGH);
    digitalWrite(buzzer, HIGH);
    digitalWrite(biru, LOW);
    digitalWrite(putih, LOW);
  } else if (cm <= 80) {
    digitalWrite(merah, HIGH);
    digitalWrite(kuning, LOW);
    digitalWrite(oranye, LOW);
    digitalWrite(buzzer, HIGH);
    digitalWrite(biru, LOW);
    digitalWrite(putih, LOW);
  } else {
    digitalWrite(merah, LOW);
    digitalWrite(kuning, LOW);
    digitalWrite(oranye, LOW);
    digitalWrite(buzzer, LOW);
    digitalWrite(biru, LOW);
    digitalWrite(putih, LOW);
  }
  
  delay(1000); // Tunggu 1 detik sebelum membaca ulang jarak
}

long mikrodetikKeCentimeter(long mikrodetik) {
  // Kecepatan suara adalah 340 m/detik atau 29 mikrodetik per cm.
  // Sinyal bergerak pergi dan kembali, jadi bagi dua untuk mendapatkan jarak.
  return mikrodetik / 29 / 2;
}

```
