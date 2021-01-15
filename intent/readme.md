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

## Intent Parcelable

### data class "DataBiografi"

> pertama buat `data class` dengan attribut yang di perlukan
>
> jangan lupa untuk penulisan data class harus menggunkan kurung biasa dan memiliki setidaknya 1 attribute

```
data class DataBiografi (
    val fullname: String? = null,
    val address: String? = null,
    val phone: String? = null
)
```

> Langkah berikutnya ketikan `: Parcelable` setelah tutup kurung
>
> nanti `Android Studio` akan memberikan *sugestion* untuk mengimport `android.os.Parcelable` <br>
lalu tekan enter untuk  pemasangan `import`
>
> Setelah proses diatas nama `data class` akan berubah menjadi merah. <br>
click kiri nama `data class` lalu tekan `alt`+`enter` lalu pilih `Add Parcelable Implementation` <br>
maka program akan membuatkan konstruksi yang dibutuhkan secara otomatis sesuai dengan data pada `data class` <br>
>
> maka pastikan terlebih dahulu bahwa telah mengisikan seluruh data pada `data class` sebelum pengimportan

### Main Activity

> penulisan pada parameter pada nama `data class` kita urutkan dengan `data class` yang kita buat<br>
pastikan bahwa urut dari atas ke bawah.

> pada program ini pengurutanya adalah 1. fullname, 2. address, 3. phone <br>
dan seluruh nya memiliki tipe data yang sama yaitu `String`

> untuk pemasangan pada `intent` sama seperti pemasangan pada `intent` yang normal <br>
kita cukup menggunakan fungsi `putExtra()` <br>
>> parameter yang pertama diisikan `String` sebagai nama variable, <br>
parameter yang kedua diisikan `parcelable` kita sebagai value yang disini adalah `dataBiografi`.

```
val dataBiografi = DataBiografi("fuad thoni, "Sidoarjo", "1234")
intent.putExtra("dataBiografi", dataBiografi)
```

### Second Activity

> sama seperti pengambilan tipe data `String`, kita bisa menggunakan fungsi `get<data-type>Extra` untuk mengambil data `intent`.
> 
> karena data yang kita kirimkan berupa `parcelable` maka kita menggunakan fungsi `getParcelableExtra()`.
>> parameter yang perlu diisikan adalah parameter pertama saat kita mengirimkan data `parcelable`
>>
>> untuk "DataBiografi" adalah nama dari `data class` kita / tipe data dari `parcelable`

```
val mParcel = intent.getParcelableExtra<DataBiografi>("dataBiografi")
val fullname = mParcel?.fullname
val address = mParcel?.address
val phone = mParcel?.phone
```


