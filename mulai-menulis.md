# Ayo! Menulis Kode

setelah nyobain **Ngrok** dan Membuat Projek Nodenya,  selanjutanya adalah, buatlah source bernama **apps.js** \(sesuai dengan entry point saat temen-temen inisialisasi node tadi \).

```js
const express = require('express');
const bodyParser = require('body-parser');
const app = express();
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
```

import module yang telah kita install di step sebelumnya.

```js
/*
Const Data Facebook Detail Apps
*/
const APP_SECRET    = "APP_SECRET_ANDA";
const VALID_TOKEN   = "VALID TOKEN ANDA";
const SERVER_URL    = "https://APAPUNYAMU.ngrok.io";
const ACCESS_TOKEN  = "PAGE ACCESS TOKEN"; 
const server = app.listen(process.env.PORT || 5000, () => {
  console.log('Express server listening on port %d in %s mode', server.address().port, app.settings.env);
})
```

dibawahnya kita mencoba tambahkan kode untuk mengetest apakah projek kita sudah jalan atau belum

```js
/* Hello Data */
app.get('/', (req, res) => {
    console.log('Server Ok!');
    res.sendStatus(200);
});
```

dan jalankan perintah tersebut , sebagia berikut :\)

```
$ node apps.js
```

kemudian, akses SERVER URLnya.![](/assets/Screen Shot 2017-09-29 at 2.34.04 PM.png)

