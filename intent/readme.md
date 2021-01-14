# Intent explicit, extra, inplicit, & parcelable

### Intent Explicit

> untuk pindah dari satu aktivity ke aktivity kita menggunakan fungsi `startActivity()`
>> fungsi `startActivity()` membutuhkan sebuah parameter `Intent` untuk pindah dari satu activity ke activity lainya.
>> 
>> untuk `ExplicitIntent` pada parameter `Intent` yang pertama memerlukan `context` activity <br>
jadi bisa ditulis `this` saja, apabila tidak bisa karena scope `this` ter tindih dengan this yang lain <br>
maka bisa menggunakan operator `@` disusul dengan nama `activity` saat menuliskan `intent`<br>
maka penulisan nya menjadi `this@MainActivity`.
>>
>> untuk parameter kedua di isikan target `class` yang akan kita singgahi.

```
binding.buttonSave.setOnClickListener {
    val intent = Intent(this, SecondActivity::class.java)
    startActivity(intent)
}
```

## Intent Extra

### putExtra
> Selain menentukan lokasi untuk berpindah, intent juga dapat membawa beberapa data saat berpindah lokasi 
>
> Cara paling sederhana untuk membawa attribut adalah dengan menggunakan fungsi `putExtra()` milik intent
>
> Penulisan `putExtra()` di tuliskan setelah penulisan `variable` yang telah diisikan target `intent`
>> parameter pertama `putExtra()` adalah string yang berfungsi sebagai `variable` saat pengambilan data di lokasi `intent` berikutnya
>>
>> parameter kedua `putExtra()` adalah value dari `varible` yang ditulis di parameter pertama.
>>> NB : untuk yang mengirim data menggunakan `variable` yang didapat dari `TextView.text` <br>
jangan lupa untuk menutupnya dengan `toString` jika tidak maka data tidak bisa diambil dengan `getStringExtra` <br>
>> karena `data type` dari `TextView.text` adalah `Editable?` bukan `String`
```
// deklarasi intent
val intent = Intent(this, ProfileActivity::class.java)

// penambahan attribut kedalam intent
intent.putExtra("fullname", fullname.toString())
intent.putExtra("address", address.toString())
intent.putExtra("phone", phone.toString())

// pengeksekusian intent
startActivity(intent)

```

### getExtra
> untuk pengambilan data kita menggunakan fungsi `.get` setelah `intent`lalu di lanjut dengan `dataType` lalu di tutup dengan `Extra` <br>
contoh untuk pengambilan data type string adalah `val data = intent.getStringExtra()`
>> didalam `get...Estra()` kita menuliskan nama variable ingin kita ambil nilainya <br> 
variable didapat dari `putExtra()` parameter pertama yang telah kita tuliskan di `activity` sebelumnya
```
val fullname = intent.getStringExtra("fullname")
val address = intent.getStringExtra("address")
val phone = intent.getStringExtra("phone")
```
