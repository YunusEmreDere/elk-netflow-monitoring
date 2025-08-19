#Proje Adı:
ELK Stack ile Netflow Trafiğini izleme

#Özellikler:
-Routerdan gelen netflow verilerini Filebeat kullanarak porttan alır
-ElastikSearch ve Kibana kullanılarak bu veriler görselleştirilir.
-ElasticSearch üzerinden filtrelemeler ile istenilen şekilde kullanılabilinir.

#Bu projeyi çalıştırmak için gerekli olanlar:
* [Docker](https://docs.docker.com/get-docker/)
* [Docker Compose](https://docs.docker.com/compose/install/)
* Git (Kodları klonlamak için)

#Kurulum:
1.  Bu depoyu (repository) yerel makinenize klonlayın:
    ```bash
    git clone https://github.com/YunusEmreDere/elk-netflow-monitoring
    ```
2.  Proje dizinine gidin:
    ```bash
    cd projeadi
    ```
3.  Servisleri başlatın:
    ```bash
    docker compose up -d
    ```

## 💻 Kullanım
-Docker konteynırı başlatıldıktan sonra Filebeat kurulumu aşamasına geçilmelidir.

1.  Filebeat indirimi ve kurulumu
    ```bash
    curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.15.0-amd64.deb
    sudo dpkg -i filebeat-8.15.0-amd64.deb
    ```
2.Konfigurasyonların yapılması
-filebeat.yml dosyasına gidilmelidir. 
    ```bash
    sudo nano /etc/filebeat/filebeat.yml
    ```
    
    ```bash
    
    reload.enabled: true
    
    reload.period: 10s
    
    setup.kibana:
    hosts: ["Your_ip_address:5601"]

    output.elasticsearch:
    hosts: ["Your_ip_address:9200"]
    ```
Olarak değiştirilmelidir.

3. Netflow modülünü etkinleştirme:
   
   ```bash
    sudo filebeat modules enable netflow
    ```

-netflow.yml dosyasına gidilmelidir.
    ```bash
    sudo nano /etc/filebeat/modules.d/netflow.yml
    ```

    ```bash
    # Module: netflow
    # Docs: https://www.elastic.co/guide/en/beats/filebeat/main/filebeat-module-net>

    - module: netflow
      log:
        enabled: true
        var:
          netflow_host: 0.0.0.0
          netflow_port: 2055
        # internal_networks specifies which networks are considered internal or p>
        # you can specify either a CIDR block or any of the special named ranges >
        # at: https://www.elastic.co/guide/en/beats/filebeat/current/defining-pro>
          internal_networks:
            - private

    ```

enabled: true, netflow_host: 0.0.0.0 ve netflow_port: (hangi portu dinleceyecekseniz.)

4.Filebeat'i başlatma:
    
    ```bash
    sudo filebeat setup
    sudo service filebeat start
    ```


5. Verilerin gelip gelmediğini kontrol etme:
   http://(your_ip_address):5601/app/home#/tutorial/netflowLogs sitesine gidip 5. aşamada test edebilirsiniz.


Daha sonrasında ElastikSearch'te dashboard'a gidip verilerinizi görebilirsiniz.

## 🤝 Katkıda Bulunma

Projenin geliştirilmesine katkıda bulunmak isterseniz, lütfen aşağıdaki adımları izleyin:

1.  Proje deposunu çatallayın (fork).
2.  Yeni bir dal (branch) oluşturun: `git checkout -b ozellik/yeni-ozellik-adi`
3.  Değişikliklerinizi yapın ve kaydedin: `git commit -m 'feat: yeni özellik eklendi'`
4.  Dalı yükleyin: `git push origin ozellik/yeni-ozellik-adi`
5.  Bir çekme isteği (pull request) açın.

## 📜 Lisans

Bu proje, MIT Lisansı ile lisanslanmıştır.

---

**Geliştirici:** Yunus Emre Dere
**Teşekkürler:** Bu projede emeği geçen herkese teşekkürler.


    

