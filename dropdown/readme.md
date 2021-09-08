
### XML
Main_activity
```
<com.google.android.material.textfield.TextInputLayout
    style="@style/Widget.MaterialComponents.TextInputLayout.FilledBox.ExposedDropdownMenu"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:hint="Select device type">

    <AutoCompleteTextView
        android:id="@+id/menuAutocompleteMode"
        android:layout_width="200dp"
        android:layout_height="wrap_content"
        android:inputType="none"
        />

</com.google.android.material.textfield.TextInputLayout>
```

list_item.xml
```
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    xmlns:tools="http://schemas.android.com/tools"
    android:ellipsize="end"
    android:maxLines="1"
    android:padding="16dp"
    tools:text="List "
    android:textAppearance="?attr/textAppearanceSubtitle1" />
```

### Kotlin
Setup item on dropdown
```
val menuAutocompleteMode = findViewById<AutoCompleteTextView>(R.id.menuAutocompleteMode)
val list_mode = listOf("a", "b", "c", "d")
val adapterMode = ArrayAdapter(this, R.layout.list_item, list_mode)
menuAutocompleteMode.setAdapter(adapterMode)
```

Set onItemClick
```
menuAutocompleteMode.setOnItemClickListener { adapterView, view, i, l ->
    var item = list_mode.get(i)
    Toast.makeText(this, "item : $item", Toast.LENGTH_SHORT).show()
}
```
