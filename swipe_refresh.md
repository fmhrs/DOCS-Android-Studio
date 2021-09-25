### Manifest
```
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```

### Dependencys
```
dependencies {
    implementation "androidx.swiperefreshlayout:swiperefreshlayout:1.1.0"
}
```

### Layout
```
<android.support.v4.widget.SwipeRefreshLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:id="@+id/swiperefresh"
    android:layout_height="match_parent"
    tools:context=".screenlistdevice.ScreenListDevice">

    <androidx.core.widget.NestedScrollView
        android:fillViewport="true"
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical">
            
            <LinearLayout
                android:visibility="gone"
                android:id="@+id/layout_no_internet"
                android:layout_width="match_parent"
                android:gravity="center"
                android:layout_height="match_parent">
                <TextView
                    android:visibility="visible"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:gravity="center_horizontal"
                    android:text="no internet connection!\nswipe ke bawah untuk memuat data" />
            </LinearLayout>
            
            
            <LinearLayout
                android:layout_width="match_parent"
                android:id="@+id/layout_activity"
                android:layout_height="match_parent"
                android:orientation="vertical">
                
                <TextView
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:text="Perangkat yang tersedia"
                    android:layout_marginHorizontal="20dp"
                    android:layout_marginTop="20dp"
                    style="@style/TextAppearance.AppCompat.Headline"/>

                <androidx.recyclerview.widget.RecyclerView
                    android:id="@+id/recyclerview"
                    android:padding="20dp"
                    android:nestedScrollingEnabled="false"
                    android:clipToPadding="false"
                    tools:listitem="@layout/recycler_screen_device"
                    android:layout_width="match_parent"
                    android:layout_height="match_parent" />

            </LinearLayout>

        </LinearLayout>

    </androidx.core.widget.NestedScrollView>

</android.support.v4.widget.SwipeRefreshLayout>
```

### Main.kt
```
onCreate(){
    myInternet = Internet(this)
        
    binding.swiperefresh.setOnRefreshListener {
        getAllDevice()
    }
}

private fun refreshLayout(){
    if(myInternet.isConnected()){
        getAllDevice()
    } else {
        disableLayout()
        Toast.makeText(this, "tidak terhubung dengan internet", Toast.LENGTH_SHORT).show()
        binding.swiperefresh.isRefreshing = false
    }
}

private fun disableLayout(){
    binding.layoutActivity.visibility = View.GONE
    binding.layoutNoInternet.visibility = View.VISIBLE
}

override fun onResume() {
    super.onResume()
    refreshLayout()
}
```

// check internet connection
### Helper -> Internet
```
import android.content.Context
import android.net.ConnectivityManager
import android.net.NetworkCapabilities
import android.os.Build

class Internet(val context: Context) {
    fun isConnected(): Boolean {

        // register activity with the connectivity manager service
        val connectivityManager = context.getSystemService(Context.CONNECTIVITY_SERVICE) as ConnectivityManager

        // if the android version is equal to M
        // or greater we need to use the
        // NetworkCapabilities to check what type of
        // network has the internet connection
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {

            // Returns a Network object corresponding to
            // the currently active default data network.
            val network = connectivityManager.activeNetwork ?: return false

            // Representation of the capabilities of an active network.
            val activeNetwork = connectivityManager.getNetworkCapabilities(network) ?: return false

            return when {
                // Indicates this network uses a Wi-Fi transport,
                // or WiFi has network connectivity
                activeNetwork.hasTransport(NetworkCapabilities.TRANSPORT_WIFI) -> true

                // Indicates this network uses a Cellular transport. or
                // Cellular has network connectivity
                activeNetwork.hasTransport(NetworkCapabilities.TRANSPORT_CELLULAR) -> true

                // else return false
                else -> false
            }
        } else {
            // if the android version is below M
            @Suppress("DEPRECATION") val networkInfo =
                connectivityManager.activeNetworkInfo ?: return false
            @Suppress("DEPRECATION")
            return networkInfo.isConnected
        }
    }
}
```
