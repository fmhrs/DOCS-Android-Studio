LORA WAN SMART WL


fragment_maps.xml
```
<?xml version="1.0" encoding="utf-8"?>
<fragment xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/map"
    android:name="com.google.android.gms.maps.SupportMapFragment"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".data.MapsFragment" />
```


activity_mac.xml
```
<fragment
    android:id="@+id/map"
    android:name="com.google.android.gms.maps.SupportMapFragment"
    android:layout_width="match_parent"
    android:layout_height="300dp">
    <ImageView android:id="@+id/imageMapTransparent"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:src="@android:color/transparent" />
</fragment>
```


MacActivity.xml
```


                       
onCreate(){
    val mapFragment = supportFragmentManager.findFragmentById(R.id.map) as SupportMapFragment
    mapFragment.getMapAsync(this@MacActivity)

    binding.imageMapTransparent.setOnTouchListener { v, event ->
        val action = event.action
        when (action) {
            MotionEvent.ACTION_DOWN -> {
                // Disallow ScrollView to intercept touch events.
                binding.scrollView.requestDisallowInterceptTouchEvent(true)
                // Disable touch on transparent view
                false
            }
            MotionEvent.ACTION_UP -> {
                // Allow ScrollView to intercept touch events.
                binding.scrollView.requestDisallowInterceptTouchEvent(false)
                true
            }
            MotionEvent.ACTION_MOVE -> {
                binding.scrollView.requestDisallowInterceptTouchEvent(true)
                false
            }
            else -> true
        }
    }

}

override fun onMapReady(googleMap: GoogleMap) {
    mMap = googleMap

    var listMarker = mutableListOf<Marker>()
    val zoomLevel = 10.0f //This goes up to 21

    val builder = LatLngBounds.Builder()

    listMac.forEachIndexed { i, dataMac->
        val maps1 = LatLng(dataMac.LATITUDE !!, dataMac.LONGITUDE!!)
        mMap.addMarker(MarkerOptions().position(maps1).title("${dataMac.NAMA_MAC}")).showInfoWindow()
        mMap.setOnInfoWindowClickListener {
            val position = Integer.parseInt(it.id.substringAfter("m"))
            val idMac = listMac[position].ID_MAC.toString()
            val nameMac = listMac[position].NAMA_MAC.toString()
            val latitude = listMac[position].LATITUDE
            val longitude = listMac[position].LONGITUDE
            val alamat = listMac[position].ALAMAT.toString()
            val foto = listMac[position].FOTO.toString()

            val intent = Intent(this, ChartActivity::class.java)
            intent.putExtra("ID_MAC", idMac)
            intent.putExtra("NAME_MAC", nameMac)
            intent.putExtra("LATITUDE", latitude)
            intent.putExtra("LONGITUDE", longitude)
            intent.putExtra("ALAMAT", alamat)
            intent.putExtra("FOTO", foto)
            startActivity(intent)
        }
        builder.include(maps1)

    }

    val bounds = builder.build()
    val padding = 100
    val cu = CameraUpdateFactory.newLatLngBounds(bounds, padding)

//        mMap.moveCamera(cu)
    mMap.animateCamera(cu)
}
```
