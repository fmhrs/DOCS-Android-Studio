# Intent implicit, explicit, & parcelable

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
