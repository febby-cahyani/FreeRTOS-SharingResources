# FreeRTOS-SharedResource-LED-Control
Proyek ini menunjukkan penggunaan FreeRTOS untuk mengelola beberapa tugas yang mencoba mengakses *shared resource* dalam sistem multitasking. Output dari program ini berkaitan dengan bagaimana keempat LED berperilaku berdasarkan pengendalian akses ke *shared resource* menggunakan mekanisme **mutual exclusion**.

Diagram Task :

![Screenshot 2024-11-17 120605](https://github.com/user-attachments/assets/8f61436f-ef1f-4106-9fb7-b93c6999653d)

Hardware yang diperlukan :
1. STM32f401CCU6
2. LED

Software yang diperlukan :
1. STM32CubeIDE
2. FreeRTOS

Cara Kerja :
1. Inisialisasi dan Penjadwalan Tugas :
   - Program dimulai dengan inisialisasi FreeRTOS yang mengonfigurasi task yang akan dijalankan secara bersamaan.
   - Ada 4 tugas utama yaitu sebagai berikut :

     a. 'GreenTask' dan 'RedTask' mencoba mengakses *shared resource" yang dinamakan 'StartFlag' menggunakan mekanisme **mutual exclusion**.
     
     b. 'OrangeTask' berjalan secara terus-menerus dengan prioritas lebih tinggi.
     
     c. 'BlueTask' akan memeriksa apakah ada konflik dalam pengaksesan *shared resource* dan mengaktifkan Led Merah jika terjadi konflik.
     
2. Mutual Exclusion (Penghindaran Konflik) :
   - Tugas 'GreenTask' dan 'RedTask' saling bergantian mengakses *shared resource* ('StartFlag'). Akses ini dilindungi menggunakan mekanisme **mutual exclusion** untuk memastikan bahwa hanya satu tugas yang dapat mengakses *shared resource* pada satu waktu.
   - Jika salah satu tugas berhasil mengakses *shared resource*, LED yang sesuai akan menyala.
   - Jika terjadi problem dan tugas gagal mendapatkan akses ke *shared resource* semisal task lain sedang mengaksesnya, 'BlueTask' akan menyala sebagai indikator terjadinya kegagalan.
     
3. LED Orange Berkedip :
   - tugas 'OrangeTask' memiliki prioritas lebih tinggi daripada task lainnya, sehingga akan selalu berjalan tanpa terganggu.
   - LED Orange 'led4' akan berkedip terus-menerus dengan delay 50ms, menunjukkan bahwa 'OrangeTask' tetap berjalan tanpa tergantung pada *shared resource*.
     
4. Penanganan Konflik :
   - Jika bagian *critical section* dihapus atau tidak diterapkan dengan benar, tugas 'GreenTask' dan 'RedTask' mungkin mencoba mengkases *shared resource* pada saat yang bersamaan, yang menyebabkan konflik.
   - 'BlueTask' akan sering menyala untuk menunjukkan adanya konflik ketika dua tugas mencoba mengakses *shared resource* pada waktu yang bersamaan.
     
5. Siklus Kerja LED :
   - **Kasus Normal** : Jika mutual exclusion bekerja dengan baik, 'GreenTask' dan 'RedTask' akan menyala bergantian, menunjukkan bahwa kedua task berhasil mengakses *shared resource*. 'OrangeTask' akan terus berkedip dengan cepat.
   - **Kasus Problem** : Jika mutual exclusion tidak diterapkan dengan bena, 'GreenTask' dan 'RedTask' mungkin menyala bersamaan atau dengan pola yang tidak teratur, dan 'BlueTask' akan menyala untuk menunjukkan adanyan konflik.

Ringkasan Perilaku LED :

![Screenshot 2024-11-17 133611](https://github.com/user-attachments/assets/a70f9518-9b90-439b-8418-d0b43b5269ed)


Pinout Hardware :

![Pinout Hardware FreeRTOS](https://github.com/user-attachments/assets/8b814f64-fa02-4ee5-a5e5-d6a85f395bb2)

Hasil Hardware :



Hasil dari proyek ini sebagai berikut :
1. **Normal**: LED Hijau dan Kuning bergantian menyala tanpa terdapat masalah, LED Merah tetap mati.
2. **Problem** : Jika dengan sengaja menghapus mekanismer *critical section*, LED Biru akan sering menyala.
   
Project ini dikerjakan di Politeknik Elektronika Negeri Surabaya dengan dosen pengampu bapak Fernando Ardilla
