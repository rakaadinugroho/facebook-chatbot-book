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

