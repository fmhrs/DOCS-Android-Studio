# Android permission
pada dasarnya android memiliki 4 jenis izin, keempat izin tersebut adalah izin normal, tanda tangan, berbahaya, dan khusus. untuk ringkasan mengenai keempat izin tersebut ada di [ringkasan izin android studio](https://developer.android.com/guide/topics/permissions/overview?hl=id).

## Meminta Izin
untuk mengakses fitur khusus seperti kamera, speaker, microphone, ataupun akses dalam folder internal hp apk android perlu meminta izin terlebih dahulu kepada pemilik hp. dalam melakukan perminitaan izin untuk mengakses fitur khusus, apk memiliki 2 buah cara
1. `android version` < 6 : akan meminta akses fitur khusus saat melakukan instalansi program.
2. `android version` >= 6 : dapat meminta akses fitur saat akses fitur tersebut di perlukan.
