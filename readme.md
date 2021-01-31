# Step by Step Command

## Step 1: Instalasi Dependencies Pada pod05-elk

ELK Stack memerlukan java untuk bisa berjalan dengan baik. Berikut ini command yang digunakan:

```
sudo apt install openjdk-11-jre apt-transport-https
```
Kemudian tekan Y untuk menyetujui proses instalasi

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Elasticsearch%20install%20%26%20configuration/1.png)

## Step 2: Instalasi & Konfigurasi Elasticsearch Pada pod05-elk

Pertama import Elasticsearch PGP key dengan command berikut ini:

```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```

Tambahkan repository untuk menginstall elasticsearch dengan command berikut:

```
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
```

Install Elasticsearch

```
sudo apt-get update && sudo apt-get install elasticsearch
```
![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Elasticsearch%20install%20%26%20configuration/2.png)

Kemudian jalankan Elasticsearch

```
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
sudo systemctl status elasticsearch
```

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Elasticsearch%20install%20%26%20configuration/3.png)

Untuk pengecekan saya menggunakan command berikut untuk mengirimkan HTTP request pada port 9200 di localhost

```
curl -X GET "localhost:9200/?pretty"
```

Yang akan memberikan output seperti ini

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Elasticsearch%20install%20%26%20configuration/4.png)

Lanjut ke konfigurasi file elasticsearch.yml

```
sudo nano /etc/elasticsearch/elasticsearch.yml
```

Kemudian edit dan uncheck bagian network.host http.port dan discovery.seed_hosts seperti berikut

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Elasticsearch%20install%20%26%20configuration/5.png)

Setelah di-save restart Elasticsearch dengan perintah berikut

```
sudo systemctl restart elasticsearch
```

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Elasticsearch%20install%20%26%20configuration/6.png)
