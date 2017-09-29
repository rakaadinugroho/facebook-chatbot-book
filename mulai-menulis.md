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

kemudian, akses SERVER URLnya.![](/assets/Screen Shot 2017-09-29 at 2.34.04 PM.png)artinya server webhook insyaallah sudah siap digunakan.

selanjutnya buat HTTP Verb \( GET \) untuk webhook untuk dipasang sebagai access token di facebook

```js
app.get('/webhook', (req, res) => {
    if (req.query['hub.mode'] && req.query['hub.verify_token'] === VALID_TOKEN) {
        res.status(200).send(req.query['hub.challenge']);
    } else {
        res.status(403).end();
    }
});
```

end-point diatas adalah untuk facebook mengecek webhook service kita sudah sesuai/siap digunakan atau belum

![](/assets/Screen Shot 2017-09-29 at 3.41.26 PM.png)

## Webhook POST

```js
/* Handling Pesan */
app.post('/webhook', (req, res) => {
    //console.log(req.body);
    if (req.body.object === 'page') {
      req.body.entry.forEach((entry) => {
        entry.messaging.forEach((event) => {
          if (event.message && event.message.text) {
            //sendMessage(event);
            console.log(event);
          }
        });
      });
      res.status(200).end();
    }
  });
```

untuk mengecek apakah saat seorang user mengirimkan pesan, akan diterima oleh webhook.

![](/assets/Screen Shot 2017-09-29 at 3.46.01 PM.png)

untuk membuat response nya, kita akan membuat fungsi **sendMessage\(event\). **kira-kira kodenya seperti dibawah:

```js
function sendMessage(event) {
    let sender = event.sender.id;
    let text = event.message.text;

    /* Test Data */
    console.log("Dikirim ke %s ", sender);

    request({
      url: 'https://graph.facebook.com/v2.6/me/messages',
      qs: {access_token: ACCESS_TOKEN},
      method: 'POST',
      json: {
        recipient: {id: sender},
        message: {text: text}
      }
    }, function (error, response) {
      if (error) {
          console.log('Error sending message: ', error);
      } else if (response.body.error) {
          console.log('Error: ', response.body.error);
      }
    });
  }
```

fungsi diatas akan mereply pesan sesuai dengan pesan yang di tuliskan oleh sender.

![](/assets/Screen Shot 2017-09-29 at 3.54.09 PM.png)

untuk nyobain end-point graph lainnya dari facebook, bisa jadi referensi disini

```
https://developers.facebook.com/tools/explorer
```

## Setup API.ai

yah! kali ini penjelasannya menggunakan api.ai, next time saya tulis versi wit.ai **:v :v :v \(**yang paham pasti ngakak**\)**

```
https://console.api.ai
```

register dan buatlah **agent** baru, isi saja nama agentnya \( yang lain biarkan default \)

