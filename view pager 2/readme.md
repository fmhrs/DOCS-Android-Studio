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

<br>

### Penulisan kotlin : recycler view (adapter)
tidak seperti recyclerview yang harus menuliskan layout manager, pada view pager kita hanya perlu memasangkan adapter ke dalam widget.
#### MainActivity
```
val listNews = mutableListOf<ModelNews>()
listNews.add(
    ModelNews(
        R.drawable.view_pager_android_studio,
        "Android Studio",
        "Android Studio adalah Integrated Development Enviroment untuk sistem operasi Android," + 
        "yang dibangun di atas perangkat lunak JetBrains IntelliJ IDEA dan didesain khusus untuk" +
        "pengembangan Android."
    )
)
...

binding.viewpager.adapter = AdapterNews(listNews)
```

#### RecyclerView adapter
pada pemubatan adapter viewpager2 bisa menggunakan berbagai macam adapter, termasuk `RecyclerView.Adpater` dan juga `FragmentStateAdapter`

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

<br>

### Penulisan kotlin : FragmentStateAdapter (adapter)
jika menggunakan `FragmentStateAdapter` sebagai gantinya `RecyclerView.Adapter` maka file kotlin yang membawa widget `viewpager2` diganti extends class nya dari `:AppCompatActivity()` menjadi `FragmentActivity()`
#### MainFragment: FragmentActivity()

```
class MainFragment: FragmentActivity() {
    private lateinit var binding : FragmentActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = FragmentActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        val listFragment = mutableListOf<Fragment>()
        listFragment.add(FragmentA())
        listFragment.add(FragmentB())

        val adapter = AdapterFragment(this, listFragment)
        binding.viewPager.adapter = adapter

        TabLayoutMediator(binding.tabLayout, binding.viewPager){ tab: TabLayout.Tab, i: Int ->
            if (i == 0) tab.text = "Fragment A"
            if (i == 1) tab.text = "Fragment B"
        }.attach()
    }
}
```


#### FragmentStateAdapter 
selain menggunakan `RecyclerView` sebagai adapter, `ViewPager` juga bisa menggunakan `FragmentStateApdapter` sebagai adapternya, namun `FragmetnStateAdapter` memiliki 3 kombinasi constructor yang bisa digunakan<br>

|Public constructors|
|-|
|`FragmentStateAdapter(FragmentActivity fragmentActivity)`|
|`FragmentStateAdapter(Fragment fragment)`|
|`FragmentStateAdapter(FragmentManager fragmentManager, Lifecycle lifecycle)`|

```
class AdapterFragment(
    fragmentActivity: FragmentActivity,
    var listFragment: MutableList<Fragment>
): FragmentStateAdapter(fragmentActivity){
    override fun getItemCount(): Int = listFragment.size
    override fun createFragment(position: Int): Fragment = listFragment[position]
}
```

## Tab Layout
`TabLayout` dapat diintegrasikan dengan `ViewPager2` dengan menggunakan `TabLayoutMediator`
### Penulisan xml
```
<com.google.android.material.tabs.TabLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_gravity="top" />
```
### Penulisan kotlin
```
TabLayoutMediator(tabLayout, viewPager) { tab, position ->
}.attach()
```

## PageTransformer
agar dapat menampilkan item `viewpager` selain item yang yang terpilih maka pada xml file ditambahkan <br>
`android:clipToPadding="false"`<br>
`android:clipChildren="false"`<br>
`android:overScrollMode="never"`

```
<androidx.viewpager2.widget.ViewPager2
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:clipToPadding="false"
    android:clipChildren="false"
    android:foregroundGravity="center"
    android:orientation="horizontal"
    android:overScrollMode="never" />
```
untuk memberikan beberapa perubahan pada tampilan `viewpager2` kita bisa memggumakan attribut `compositePageTransformer`.

```
val compositePageTransformer = CompositePageTransformer()
compositePageTransformer.addTransformer(MarginPageTransformer(120))
compositePageTransformer.addTransformer(transformer(listNews.size))

binding.viewpager.setPageTransformer(compositePageTransformer)
```
`PageTransformer` bisa di kombinasikan kedalam `addTransformer()` namun bisa juga langsung di padsang kedalam `viewpager.setPageTransformer()`
```
inner class transformer(val listSize: Int): ViewPager2.PageTransformer{
    override fun transformPage(page: View, position: Float) {
        if(binding.viewpager.currentItem == 0){
            page.setTranslationX(-80f)
        } else if (binding.viewpager.currentItem == listSize-1){
            page.setTranslationX(80f)
        } else {
            page.setTranslationX(0f);
        }
    }
}
```

### Circle indicator can visite [this page](https://medium.com/@adrian.kuta93/android-viewpager-with-dots-indicator-a34c91e59e3a)
