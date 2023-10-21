# Integrasi Apache Superset dengan MongoDB

Dalam kasus ini pada dasarnya Apache Superset tidak mendukung NoSQL secara langsung seperti MongoDB, namun Apache Superset mendukung Apache Drill yang artinya kita bisa menyambungkan Apache Drill dengan MongoDB dan selanjutnya Apache Drill disambungkan dengan Apache Superset.

## Configuration Apache Superser

- Ubah SECRET_KEY pada file ./superset/superset_config.py
- Ubah akses root Apache Superset di dalam environment docker-compose.yaml

Start Apache Superset & Apache Drill
```docker
docker compose build
docker compose up -d
```
Apache Superset akan berjalan pada port 8088 (http://localhost:8088)

Stop Apache Superset & Apache Drill
```docker
docker compose down
```

## Configurasi Apache Drill

Apache Drill adalah Schema-free SQL Query Engine for Hadoop, NoSQL and Cloud Storage.

Apache Drill akan berjalan pada port 8047 (http://localhost:8047)

Pengaturan Awal Apache Drill
- Buka Menu Storage
- Enable Mongo Plugin
- Update dan tambahkan:

  ```json
  {
    "type": "mongo",
    "connection": "mongodb://localhost:27017/",
    "pluginOptimizations": {
        "supportsProjectPushdown": true,
        "supportsFilterPushdown": true,
        "supportsAggregatePushdown": true,
        "supportsSortPushdown": true,
        "supportsUnionPushdown": true,
        "supportsLimitPushdown": true
    },
    "batchSize": 100,
    "enabled": true,
    "authMode": "SHARED_USER"
  }
  ```
- Buka menu Options dan ubah menjadi:
  ```sh
  drill.exec.functions.cast_empty_string_to_null : true
  store.mongo.all_text_mode : true
  store.mongo.bson.record.reader : false
  ```
## Menyambungkan Apache Drill ke Apache Superset

- Buka Data pada pojok kanan atas
- Pilih connect Database
- Pilih Support Database -> Apache Drill
- Tambahkan SQLALCHERY URI -> drill+sadrill://drill:8047/mongo?use_ssl=False

Refereny:
- [MongoDB to Apache Drill to Apache Superset]( https://www.shubhamdipt.com/blog/mongodb-to-apache-drill-to-apache-superset/)
- [Connecting to Databases Apache Drill](https://superset.apache.org/docs/databases/drill/)
- [Official Docker Hub Apache Superset](https://hub.docker.com/r/apache/superset)
- [Official Docker Hub Apache Drill](https://hub.docker.com/r/apache/drill)

