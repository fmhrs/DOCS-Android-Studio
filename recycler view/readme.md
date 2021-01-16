# Recycler view
### Lankah - langkah
1. pasang Recycler View di `MainActivity.xml`
2. buat tampilan recycler view `AdapterMurid.xml`
3. buat `data class` buat data di tampilan `AdapterMurid.xml`
4. buat dan Recycler View Adapter dengan membuat class `AdapterMurid.kt`yang extends dengang `RecyclerView.Adapter<VH>()`
5. pasang Adapter `AdapterMurid.kt` kedalam Recycler View pada `MainActivity.kt`

### Main XML
> untuk pemasangan Recycler View apabila overflow tidak perlu menambahkan `ScrollView`
```
<androidx.recyclerview.widget.RecyclerView
    android:id="@+id/recyclerview"
    android:layout_marginTop="24dp"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

### AdapterMurid XML
> `AdapterMurid.xml` ini adalah tampilan yang nantinya akan di gunakan berulang kali dalam penggunaan RecyclerView
>
> xml di bawah hanyalah contoh, yang terpenting ada pada id attribut <br>
id yang di gunakan disini adalah `nama`, `alamat`, dan juga `telpon`
>

> Disini kita menggunakan attribute `Material Design`, jadi untuk Versi Android Studio < Versi 4.1 <br>
diwajibkan untuk melakukan implementasi komponen `Material Design`
>> informasi lebih lengkap mengenai `Material Design` dapat mengunjungi [material.io](https://material.io/develop/android)
```
<androidx.cardview.widget.CardView
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginBottom="12dp"
    android:elevation="12dp"
    app:cardUseCompatPadding="true"
    app:cardCornerRadius="8dp">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="14dp"
        android:orientation="vertical">

        <com.google.android.material.textview.MaterialTextView
            style="@style/TextAppearance.AppCompat.Body1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Nama"
            android:textColor="@android:color/secondary_text_light_nodisable" />

        <com.google.android.material.textview.MaterialTextView
            android:id="@+id/nama"
            style="@style/TextAppearance.AppCompat.Large"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="nama_murid" />

        <com.google.android.material.textview.MaterialTextView
            style="@style/TextAppearance.AppCompat.Body1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="6dp"
            android:text="Alamat"
            android:textColor="@android:color/secondary_text_light_nodisable" />

        <com.google.android.material.textview.MaterialTextView
            android:id="@+id/alamat"
            style="@style/TextAppearance.AppCompat.Large"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="alamat_murid" />

        <com.google.android.material.textview.MaterialTextView
            style="@style/TextAppearance.AppCompat.Body1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="6dp"
            android:text="Nomor Handphone"
            android:textColor="@android:color/secondary_text_light_nodisable" />

        <com.google.android.material.textview.MaterialTextView
            android:id="@+id/telpon"
            style="@style/TextAppearance.AppCompat.Large"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Nomor_Hanphone" />
    </LinearLayout>
</androidx.cardview.widget.CardView>
```

### data class murid
> `data class` ini berguna sebagai pengirim data dari `MainActivity.kt` ke `AdapterMurid.kt`
```
data class DataMurid(
    val nama: String? = null,
    val alamat: String? = null,
    val telpon: String? = null,
)
```

### AdapterMurid Kotlin
> `val listMurid` adalah `data class` murid berbentuk list yang dikirim dari `MainActivity.kt`<br>
atau file kotlin yang menggunakan adapter `MuridAdapter.kt`


> buat class extends `RecyclerView.Adapter<VH>()`
> 
> buat inner class yang extends `RecyclerView.ViewHolder()`
>
> lalu masukkan nama innerclass tersebut kedalam `<VH>`
>
>> `<VH>` adalah inisialisasi dari view holder 

> `RecylcerView.Adapter` mengharuskan kita mengimplementasikan 3 function
>> `onCreate` pembentukan tampilan pada android
>> 
>> `onBindViewHolder` configurasi attribut pada xml
>>
>> `getItemCount` jumlah perulangan tampilan xml yang akan di tampilkan pada `<recyclerview>` di `MainActivity.kt`

> warning : penulisan `MuridAdapter` dibawah menggunakan data binding
>> untuk pengaktifan data binding bisa lihat [di sini](https://github.com/fmhrs/android-studio-code/blob/master/view%20binding/readme.md)
```
class MuridAdapter(
    val listMurid: List<DataMurid>
) : RecyclerView.Adapter<MuridAdapter.ViewHolder>() {
    inner class ViewHolder(binding: AdapterMuridBinding) : RecyclerView.ViewHolder(binding.root)

    lateinit var binding: AdapterMuridBinding
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        binding = AdapterMuridBinding.inflate(LayoutInflater.from(parent.context), parent, false)
        return ViewHolder(binding)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        binding.nama.text = listMurid[position].nama
        binding.alamat.text = listMurid[position].alamat
        binding.telpon.text = listMurid[position].telpon
    }

    override fun getItemCount(): Int = listMurid.size
}
```


### MainActivity Kotlin
> `listMurid` adalah tempat data yang akan di tampilkan ke dalam Recycler View 
>
> Setup Recycler View kita hanya memerlukan `adapter` dan `layoutManager`
>> `linearManager`disini kita hanya perlu mengisikan dengan `LinearLayoutManager()` <br>
dengan parameter context jadi cukup dituliskan dengan `this`. <br>
>>> apabila penggunaan `this` tidak bekerja karena yang terakses adalah `this` yang lain bukan `this` context <br>
maka kita dapat menggunakan `@` setelah `this` lalu disusul dengan nama activity kita. ex : `this@MainActivity`
>>
>> `adapter` kita hanya perlu mengisikan nama adapter yang telah kita buat <br>bersama dengan data yang akan di gunakan oleh adapter
```
    val listMurid= listOf<DataMurid>(
        DataMurid("Fuad Mahrus Fathoni", "taman sidoarjo", "0123321123"),
        DataMurid("M Darojatun Hogi", "kureksari waru", "08123987312"),
        DataMurid("Ardeno Rama", "candi sidoarjo", "03123456234"),
        DataMurid("Fuad Mahrus Fathoni", "taman sidoarjo", "0123321123"),
        DataMurid("M Darojatun Hogi", "kureksari waru", "08123987312"),
        DataMurid("Ardeno Rama", "candi sidoarjo", "03123456234")
    )

    binding.recyclerview.layoutManager = LinearLayoutManager(this)
    binding.recyclerview.adapter = MuridAdapter(listMurid)
```
