# ChatBot with Facebook Messenger

Membangun Chatbot pada Platform Facebook Messenger dengan NodeJs, dan sedikit ramuan API.ai biar nggak kaku chatbotnya

Tools yang diperlukan:

* NodeJs
* NgRok Server
* Code Editor \( Terserah\)

![](/assets/Screen Shot 2017-09-29 at 4.31.06 PM.png)

nah, sekarang chatbotnya sudah manusiawi.

next challenge, menambahkan 3rd party wheater di chatbot.

## Setting Intent AI

![](/assets/Screen Shot 2017-09-29 at 4.46.22 PM.png)buka dan buatlah **intent** didashboard **api.ai**

![](/assets/Screen Shot 2017-09-29 at 4.49.28 PM.png)

kita contohkan, pada user says : **how is the weather today in jakarta?** sebagai parameter adalah **jakarta**, nah blok jakarta dan cari **geoCity** .

![](/assets/Screen Shot 2017-09-29 at 4.55.51 PM.png)

intent digunakan untuk mencari maksud dari user \( apa yang ingin dilakukan \) action digunakan untuk mengambil geo-city\) dan response adalah ketika tidak ada nama kota yang di maksudkan.

setelah itu, save **Intent** yang telah dibuat. lanjutnya, buat Webhook untuk 3rd API nya.

buka **Fulfillment**



