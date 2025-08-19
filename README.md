# ELK Stack ile Netflow Trafik İzleme

Bu depo, Docker Compose kullanarak Elasticsearch ve Kibana'yı kurmak ve Filebeat aracılığıyla ağınızdaki NetFlow trafiğini izlemek için gerekli konfigürasyonları içerir.

**Not:** Bu kurulumda Elasticsearch ve Kibana Docker konteynerleri içinde, Filebeat ise ana makineye kurulmuştur.

---

### Özellikler
* Router'dan gelen NetFlow verilerini Filebeat kullanarak porttan alır.
* Elasticsearch ve Kibana kullanılarak bu veriler görselleştirilir.
* Kibana üzerinden filtrelemeler ile veriler istenilen şekilde incelenebilir.

---

### Gereksinimler
* [**Docker**](https://docs.docker.com/get-docker/)
* [**Docker Compose**](https://docs.docker.com/compose/install/)
* Git (Kodları klonlamak için)

---

### Kurulum

1.  Bu depoyu yerel makinenize klonlayın:
    ```bash
    git clone [https://github.com/YunusEmreDere/elk-netflow-monitoring](https://github.com/YunusEmreDere/elk-netflow-monitoring)
    ```

2.  Proje dizinine gidin:
    ```bash
    cd elk-netflow-monitoring
    ```

3.  Servisleri başlatın:
    ```bash
    docker compose up -d
    ```

### Kullanım

1.  **Filebeat İndirme ve Kurulumu:**
    Ana makinenize Filebeat'i indirin ve kurun.
    ```bash
    wget [https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.15.0-amd64.deb](https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.15.0-amd64.deb)
    sudo dpkg -i filebeat-8.15.0-amd64.deb
    ```

2.  **Konfigürasyonları Yapma:**
    Filebeat konfigürasyon dosyasını açın. Eğer Kibana ve Elasticsearch'e şifre koymadıysanız, aşağıdakileri dosyanın sonuna ekleyin.
    ```bash
    sudo nano /etc/filebeat/filebeat.yml
    ```
    ```yaml
    # ... dosyanın sonuna ekleyin

    setup.kibana:
      hosts: ["localhost:5601"]

    output.elasticsearch:
      hosts: ["localhost:9200"]
    ```

3.  **NetFlow Modülünü Etkinleştirme:**
    NetFlow modülünü etkinleştirin.
    ```bash
    sudo filebeat modules enable netflow
    ```
    Ardından `netflow.yml` dosyasını açarak NetFlow veri akışını dinleyeceği portu belirtin.
    ```bash
    sudo nano /etc/filebeat/modules.d/netflow.yml
    ```
    ```yaml
    - module: netflow
      log:
        enabled: true
        var:
          netflow_host: 0.0.0.0
          netflow_port: 2055
    ```

4.  **Filebeat'i Başlatma:**
    Gerekli panoları ve şablonları yükleyin, ardından Filebeat servisini başlatın.
    ```bash
    sudo filebeat setup
    sudo systemctl start filebeat
    sudo systemctl enable filebeat
    ```

5.  **Verileri Kontrol Etme:**
    Tarayıcınızda `http://localhost:5601` adresine giderek Kibana'ya erişin. "Dashboards" bölümünden verilerinizi görüntüleyebilirsiniz.

---

### Katkıda Bulunma

Projenin geliştirilmesine katkıda bulunmak isterseniz, lütfen aşağıdaki adımları izleyin:

1.  Proje deposunu çatallayın (fork).
2.  Yeni bir dal (branch) oluşturun: `git checkout -b ozellik/yeni-ozellik-adi`
3.  Değişikliklerinizi yapın ve kaydedin: `git commit -m 'feat: yeni özellik eklendi'`
4.  Dalı yükleyin: `git push origin ozellik/yeni-ozellik-adi`
5.  Bir çekme isteği (pull request) açın.

---

### Lisans

Bu proje, MIT Lisansı ile lisanslanmıştır.

---

**Geliştirici:** Yunus Emre Dere
