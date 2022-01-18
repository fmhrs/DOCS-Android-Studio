# Json tutorial
JSON, singkatan dari JavaScript Object Notation, adalah suatu format ringkas pertukaran data komputer. Formatnya berbasis teks dan terbaca-manusia serta digunakan untuk merepresentasikan struktur data sederhana dan larik asosiatif [sumber](www.discord.com). JSON bisa di gunakan oleh banyak bahasa pemrograman sehingga sering di gunakan sebagai perantara untuk mentransfer data antar perangkat seperti Webiste dengan Android.

### Install GSON
Tambahkan code berikut pada Directory `Gradle Script > build.gradle` untuk menginstal library.
```gradle
dependencies {
  implementation 'com.google.code.gson:gson:2.8.6'             // cara 1
  implementation 'com.squareup.retrofit2:converter-gson:2.9.0' // cara 2
}
```
Tambahkan kode berikut di awal pada file yang akan kalian gunakan fungsi library GSON nya.
```kotlin
import com.google.gson.JsonParser
```

### JSON Parse
Mengubah nilai String JSON sebagai objek.
```kotlin
val jsonObject = JsonParser().parse(message.toString()).asJsonObject
```

### Get JSON Object
Mengambil nilai dari JSON objek dalam bentuk String
```kotlin
val status = jsonObject.get("s").asString  // cara1
val status = jsonObject["s"].asString      // cara2
```

### Mengambil nilai dari format json yang berbeda `important`
Misal ada 3 buah format json yang di ambil berbeda dalam 1 function JSON Parserer yang sama maka harus di check apakah JSON tersebut memiliki objek yang di ambil atau tidak sebelum menggunakan fungsi `get()` jika tidak dan program menjalankan fungsi `get()` dengan objek yang tidak tersedia maka line di bawahnya tidak akan di jalankan!
Untuk memeriksa apakah objek memiliki objek yang kita cari maka kita dapat menggunakan fungsi `has()`.
#### Contoh : ada 3 jenis json yang berbeda yang akan di kirim
1. `{"s":"p", "a":7, "b":8}`
2. `{"hnd":"L", "c":true}`
3. `{"bot":"T", "c":true}`

Dari 3 JSON yang dikirim di atas kita mengambil objek `"s"`, `"hnd"`, dan `"bot"` sebagai patokan objek yang akan di eksekusi.
```kotlin
/* !IMPORTANT!
jika langsung jsonObject["s"].asString dan tidak ada objek `s` maka program akan force break */

// jika objek tersedia maka ambil objek jika tidak maka return null
val s:String ?= if(jsonObject.has("s")) jsonObject["s"].asString else null
val hnd:String ?= if(jsonObject.has("`"s"`")) jsonObject["`"s"`"].asString else null
val bot:String ?= if(jsonObject.has("bot")) jsonObject["bot"].asString else null

if (s == "r") {
    // execute format JSON 1 here
} else if (hnd == "L") {
    // execute format JSON 2 here
} else if (bot == "T") {
    // execute format JSON 3 here
}
