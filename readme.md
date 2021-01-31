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
sudo apt-get update && sudo apt-get install elasticsearch -y
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

## Step 3: Instalasi dan Konfigurasi Kibana pada pod05-elk

Gunakan command berikut untuk melakukan instalasi

```
sudo apt-get install kibana -y
```

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Kibana%20install%20%26%20configuration/1.png)

Konfigurasi parameter server.hosts di Kibana.yml

```
sudo nano /etc/kibana/kibana.yml
```

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Kibana%20install%20%26%20configuration/2.png)

Kemudian jalankan Kibana dengan command berikut

```
sudo systemctl enable kibana
sudo systemctl start kibana
sudo systemctl status kibana
```

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Kibana%20install%20%26%20configuration/3.png)

Gunakan netstat untuk melihat port Elasticsearch dan Kibana apakah sudah berjalan dengan baik

```
sudo apt install net-tools -y
netstat -tulpn | grep "9200\|5601"
```

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Kibana%20install%20%26%20configuration/4.png)

Untuk akses tampilan web Kibana menggunakan port 5601 yang bisa diakses melalui ssh tunnel dengan command berikut

```
ssh -v -L 5601:127.0.0.1:5601 student@138.201.60.4 -p 10510
```

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Kibana%20install%20%26%20configuration/5.png)

Kemudian buka halaman website http://localhost:5601 pada browser

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Kibana%20install%20%26%20configuration/6.png)

## Step 4: Instalasi Logstash pada pod05-elk

Gunakan command berikut untuk melakukan instalasi

```
sudo apt install logstash -y
```

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Logstash%20installation/1.png)

Kemudian jalankan Logstash dengan perintah berikut

```
sudo systemctl start kibana
sudo systemctl status kibana
```

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Logstash%20installation/2.png)

## Step 5: Instalasi & Konfigurasi Metricbeat pada pod05-client

Gunakan command berikut untuk melakukan instalasi

```
curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.10.2-amd64.deb
sudo dpkg -i metricbeat-7.10.2-amd64.deb
```

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Metricbeat%20install%20%26%20configuration/1.png)

Kemudian konfigurasi file metricbeat.yml untuk mengkoneksikan Metricbeat ke Kibana & Elasticsearch

```
sudo nano /etc/metricbeat/metricbeat.yml
```
Konfigurasi Kibana host

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Metricbeat%20install%20%26%20configuration/2.png)

Konfigurasi Elasticsearch host

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Metricbeat%20install%20%26%20configuration/3.png)

Selanjutnya load asset Metricbeat menggunakan command berikut

```
sudo metricbeat setup -e
```

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Metricbeat%20install%20%26%20configuration/4.png)

Dan tunggu sampai output loaded dashboard pastikan Elasticsearch & Kibana sudah berjalan pada pod05-elk

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Metricbeat%20install%20%26%20configuration/5.png)

Jalankan Metricbeat

```
sudo systemctl enable metricbeat
sudo systemctl start metricbeat
sudo systemctl status metricbeat
```

Untuk melihat hasil dashboard yang sudah di load buka halaman http://localhost:5601/app/kibana#/dashboard/Metricbeat-system-overview-ecs. Dashboard ini menampilkan hasil monitoring pada pod05-client secara instan tanpa perlu membuat dashboard.

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Metricbeat%20install%20%26%20configuration/6.png)

## Step 6: Membuat Dashboard dan Visualisasi Penggunaan Resource pod05-client Pada Kibana

Pada bagian navigasi klik dashboard untuk beralih ke menu dashboard

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/1.png)

Di menu dashboard klik create dashboard

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/2.png)

Kemudian klik create new untuk membuat visualisasi

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/3.png)

Pada new visualization pilih metric sebagai template visualisasi

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/4.png)

Untuk source-nya pilih metricbeat-* untuk mendapatkan metrik yang ada pada pod05-client

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/5.png)

Pada bagian "Data>Metric>Aggregation" gunakan max dan pada bagian field ketikan system.uptime.duration.ms untuk custom label sesuaikan. Kemudian klik update lalu save

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/6.png)

Sesuaikan bagian title kemudian klik save & return

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/7.png)

Berikut adalah panel yang sudah dibuat untuk memonitor Up Time pada pod05-client

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/8.png)

Klik save dashboard kemudian berikan title dan klik save untuk menyimpan hasil visualisasi pada dashboard

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/10.png)

Lanjut untuk membuat panel jumlah core prosesor pod05-client dengan mengklik edit pada dashboard yang sudah di save kemudian sesuaikan seperti gambar berikut

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/11.png)

Untuk menghilangkan margins diantara panel klik option kemudian uncheck "Use margins between panels"

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/12.png)

Kemudian tambahkan beberapa panel seperti digambar berikut

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/13.png)

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/14.png)

Untuk menambahkan informasi inbound traffic bisa menggunakan panel yang sudah jadi yang di load dari Metricbeat dengan cara mengklik add kemudian search keyword inbound kemudian klik opsi yang ada [Metricbeat System]

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/15.png)

Untuk panel outbound traffic

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/16.png)

Berikut adalah hasil panel yang ditambahkan

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/17.png)

Kemudian tambahkan beberapa panel lagi

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/18.png)

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/19.png)

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/20.png)

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/21.png)

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/22.png)

Berikut adalah hasil panel yang baru ditambahkan pada baris kedua dashboard ini

![](https://github.com/jhodys/elk-stack/blob/main/Screenshots/Visualization%20of%20pod05-client%20resource%20usage/23.png)
