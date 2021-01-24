# Android permission
pada dasarnya android memiliki 4 jenis izin, keempat izin tersebut adalah izin normal, tanda tangan, berbahaya, dan khusus. untuk ringkasan mengenai keempat izin tersebut ada di [ringkasan izin android studio](https://developer.android.com/guide/topics/permissions/overview?hl=id).

## Deklarasi izin
untuk meminta izin ke pengguna, diwajibkan untuk menuliskan terlebih dahulu izin apa yang akan dimita dan di gunakan saat apk berjalan. tanpa adanya deklarasi code unduk meminta izin tidak akan berjalan dengan semestinya. untuk pendeklarasian izin dapat di tuliskan langsung di dalam `AndroidManifest.xml`

```
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.practice">

    <uses-permission android:name="android.permission.CAMERA"/>
    <uses-permission android:name="android.permission.INTERNET"/>
    
    <application
       ...
    </application>
</manifest>
```

## Meminta Izin
untuk mengakses fitur khusus seperti kamera, speaker, microphone, ataupun akses dalam folder internal hp apk android perlu meminta izin terlebih dahulu kepada pemilik hp. dalam melakukan perminitaan izin untuk mengakses fitur khusus, apk memiliki 2 buah cara
1. `android version` < 6 : akan meminta akses fitur pada saat melakukan instalansi program.
2. `android version` >= 6 : akan meminta akses fitur pada saat fitur diperlukan untuk digunakan saja.

### Meminta izin
Untuk penulisan code dalam meminta izin penggunaan fitur dapat menggunakan code `ActivityCompapt.requestPermission()`

parameter yang diperlukan dalam fungsi `requestPermission()` adalah
1. `context` : bisa dituliskan dengan `this` apabila berada dalam activity
2. `array<String>` : penulisan `String` dituliskan dengan `android.Manifest.permission.` lalu nama izin yang akan di gunakan seperti `INTERNET`. bentuk array disini berfungsi untuk melakukan berbagai macam permintaan izin dalam satu kali izin. nb: nilai array tidak bisa `null` atau `empty`
3. `int` : dapat diisikan dnegan angka `random` berfungsi sebagai pembeda hasil permintaan izin satu dengan yang lain.

```
    // pada arrayOf dapat ditambahkan izin selain camera bilau perlu, contohnya seperti
    //arrayOf(android.Manifest.permission.CAMERA, android.Manifest.permission.INTERNET).
    ActivityCompat.requestPermissions(this, arrayOf(android.Manifest.permission.CAMERA), 0)
```

### Izin telah diberikan
untuk melihat apakah izin untuk sebuah fituer telah di berikan atau belum dapat di lakukan pengecekan menggunakan fungsi `checkSelfPermission()` dengan parameter `String` izin yang ingin di periksa, lalu bandingkan `checkSelfPermission` dengan `PackageManager.PERMISSION_GRANTED`
```
if (checkSelfPermission(android.Manifest.permission.CAMERA) == PackageManager.PERMISSION_GRANTED) {
    // jika camera telah memiliki izin untuk di gunakan maka code ini akan berjalan
    Log.d(TAG, "checkCameraPermission: camera has granted");
}
```

### Contoh pengkodean
```
private fun requestPermission() {
    var listPermission = mutableListOf<String>()
    if (checkSelfPermission(android.Manifest.permission.CAMERA) == PackageManager.PERMISSION_GRANTED) {
        Log.d(TAG, "checkCameraPermission: camera has granted");
    } else {
        listPermission.add(android.Manifest.permission.CAMERA)
    }
    if (checkSelfPermission(android.Manifest.permission.INTERNET) == PackageManager.PERMISSION_GRANTED) {
        Log.d(TAG, "checkCameraPermission: internet has granted");
    } else {
        listPermission.add(android.Manifest.permission.INTERNET)
    }
    if(listPermission.isNotEmpty()){
        ActivityCompat.requestPermissions(this, listPermission.toTypedArray(), 0)
    }
}
```
