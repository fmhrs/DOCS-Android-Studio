# View Pager 2
apabila scroll view digunakan untuk menggulir tampilan, view pager 2 akan stop bergulir saat menemukan tamplian baru, dan view pager 2 dapat di implementasikan dengan TabLayout

<br>

## Pemasangan viewpager2

### Penulisan xml
utnuk melakukan pemasangan viewpager pada android studio, sama seperti pemasangan `button` atau widget lainya, viewpager harus menuliskan widget terdebut kedalam xml
```
<androidx.viewpager2.widget.ViewPager2
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:foregroundGravity="center"
    android:orientation="horizontal"
    android:overScrollMode="never" />
```

### Penulisan kotlin
tidak seperti recyclerview yang harus menuliskan layout manager, pada view pager kita hanya perlu memasangkan adapter ke dalam widget.
```
val listNews = mutableListOf<ModelNews>()
listNews.add(
    ModelNews(
        R.drawable.view_pager_android_studio,
        "Android Studio",
        "Android Studio adalah Integrated Development Enviroment untuk sistem operasi Android, yang dibangun di atas perangkat lunak JetBrains IntelliJ IDEA dan didesain khusus untuk pengembangan Android."
    )
)
...

binding.viewpager.adapter = AdapterNews(listNews)
```

### Pembuatan adapter
pada pemubatan adapter kita bisa menggunakan berbagai macam adapter, termasuk `RecyclerView.Adpater` dan juga `FragmentStateAdapter`

#### RecyclerView.Adapter
```
class AdapterNews(
    val listNews: MutableList<ModelNews>
): RecyclerView.Adapter<AdapterNews.ViewHolder>() {
    private lateinit var binding: AdapterNewsBinding
    inner class ViewHolder(binding: AdapterNewsBinding):RecyclerView.ViewHolder(binding.root)

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        binding = AdapterNewsBinding.inflate(LayoutInflater.from(parent.context), parent, false)
        return ViewHolder(binding)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        binding.imageView.setImageResource(listNews[position].image)
        binding.textViewTitle.text = listNews[position].title
        binding.textViewDescription.text = listNews[position].description
    }

    override fun getItemCount(): Int = listNews.size
}
```
#### FragmentStateAdapter (onProgress)
