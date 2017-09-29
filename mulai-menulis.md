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

untuk membuat response nya, kita akan membuat fungsi **sendMessage\(event\). **pertama import dulu nih module menjalankan request.

```
npm install request --save
```

dan tambahkan kode dibawah ini, di baris deklarasi konstanta:

```
const request = require('request');
```

lanjut, buatlah fungsi **sendMessage , **kira-kira kodenya seperti dibawah:

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

setelah itu, buat/aktifkan **SmartTalk** untuk kasus ~~salah beli ~~chatBot yang tidak kaku, bisa juga ditest untuk responsenya seperti apa ? \(belum support indonesian\)

![](/assets/Screen Shot 2017-09-29 at 4.09.01 PM.png)

setelah itu, install module API AI untuk NodeJs

```
npm install apiai
```

referensinya ada disini

```
https://www.npmjs.com/package/apiai
```

import module API AInya kedalam **apps.js**

```
const API_TOKEN = "39e70b551e0140d5bd9debf1b022e2b0";
const aiApp    = require('apiai')(API_TOKEN);
```

setelah itu, kita akan coba rombak function **sendMessage** menjadi seperti ini

```
function sendMessage(event) {
    let sender = event.sender.id;
    let text = event.message.text;

    let apiai = aiApp.textRequest(text, {
      sessionId: 'anakjalananbro'
    });

    apiai.on('response', (response) => {
      // Response ke Facebook messenger
    });

    apiai.on('error', (error) => {
      console.log(error);
    });

    apiai.end();
  }
```

pada bagian komentar **// Response ke facebook messenger** kita akan mengambil response dari **API.ai** tersebut, dengan skema Response \( JSON \) seperti gambar diatas.

```
let aiText = response.result.fulfillment.speech;
```

tambahkan di respnse apiai.on response dengan kode ini

```
let aiText = response.result.fulfillment.speech;
console.log("response ai %s ", aiText);
request({
    url: 'https://graph.facebook.com/v2.6/me/messages',
    qs: {access_token: ACCESS_TOKEN},
    method: 'POST',
    json: {
        recipient: {id: sender},
        message: {text: aiText}
    }
}, (error, response) => {
    if (error) {
        console.log('Error sending message: ', error);
    } else if (response.body.error) {
        console.log('Error: ', response.body.error);
    }
});
```

kemudian jalan kan lagi appsnya, dan coba chat dengan pages yang telah dibuat menggunakan bahasa inggris

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

buka **Fulfillment **dan buat **Webhook** yang akan kita buat dengan end-point [http://url.ngrok.io/ai](http://url.ngrok.io/ai)

jangan lupa untuk register di **OpenWeatherMap**, contoh Responsenya seperti dibawah ini.

```
{
    "coord": {
        "lon": 106.85,
        "lat": -6.21
    },
    "weather": [{
        "id": 802,
        "main": "Clouds",
        "description": "scattered clouds",
        "icon": "03d"
    }],
    "base": "stations",
    "main": {
        "temp": 303.15,
        "pressure": 1009,
        "humidity": 74,
        "temp_min": 303.15,
        "temp_max": 303.15
    },
    "visibility": 9000,
    "wind": {
        "speed": 5.1,
        "deg": 50
    },
    "clouds": {
        "all": 40
    },
    "dt": 1506679200,
    "sys": {
        "type": 1,
        "id": 8043,
        "message": 0.0271,
        "country": "ID",
        "sunrise": 1506638306,
        "sunset": 1506682037
    },
    "id": 1642911,
    "name": "Jakarta",
    "cod": 200
}
```

setelah itu, tambahkan end-point untuk **AI nya**,

```
const WEATHER_API_KEY   = "WEATHERKEY";
app.post('/ai', (req, res) => {
    if (req.body.result.action === 'weather') {
        let city = req.body.result.parameters['geo-city'];
        console.log(city);
        let restUrl = 'http://api.openweathermap.org/data/2.5/weather?APPID='+WEATHER_API_KEY+'&q='+city;
        request.get(restUrl, (err, response, body) => {
            if (!err && response.statusCode == 200) {
                let json = JSON.parse(body);
                let msg = json.weather[0].description + ' and the temperature is ' + json.main.temp + ' ℉';
                return res.json({
                  speech: msg,
                  displayText: msg,
                  source: 'weather'});
              } else {
                return res.status(400).json({
                  status: {
                    code: 400,
                    errorType: 'I failed to look up the city name.'}});
              }
        });
    }
  });
```

dan Hasil akhirnya adalah

![](/assets/Screen Shot 2017-09-29 at 5.32.29 PM.png)

