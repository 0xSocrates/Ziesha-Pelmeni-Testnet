<h1 align="center"> Ziesha | Pelmeni Testnet </h1>

![1500x500](https://user-images.githubusercontent.com/108215275/215424439-484b5e5d-c0b0-4ab5-9761-5f762ebcfc9c.jpg)

# Notlar
## Testnete biden fazla şekilde katılma imkanı var
* Sadece node çalıştımak
* Node çalıştırmak ve aynı zamanda madencilik havuzuna katılmak.
* Solo madencilik.
* Madencilik havuzu kurmak.
## Ziesha yazılım araçları:
* Bazuka: Ziesha protokolü için wallet ve node yazılımı.
* Zoro: MPN devreleri ve işlem yürütücü yazılım. Solo madencilik yapanlar ve madencilik havuzu kuranlar kullanır.
* Uzi-Pool: Madencilik havuzu oluşturma ve yönetme yazılımı. Sadece havuz sahipleri çalıştırır.
* Uzi-Miner: PoW (RandomX) bulmaca çözücü. Madencilik yapan herkes çalıştırır.
* Daha ayrıntılı bilgi için Ziesha'nın [Github](https://github.com/ziesha-network) hesabına bakabilirsiniz

## Sistem gereksinimleri:
* Sadece node (Bazuka)  çalıştırmak için minimum 2CPU 2GB RAM 20GB SSD yeterli
* Zoro için ekran kartı (GPU) ve minimum 32GB RAM gerekiyor
* Uzi-Miner için minimum 4GB RAM'e daha ihtiyacınız olacak CPU içinse herhangi bir sınırlama yok. işlemci gücünüz ne kadar yüksekse blok bulma ihtimali  ve dolayısı ödül miktarı da o oranda yüksek olacaktır.

## Eğer daha önceki testnete katıldıysanız (Groth-Testnet) aynı cüzdanın mnemoniclerini kullanın
## Bu testnette cüzdan işlemleri için daha çok Zieshanın yeni geliştirdiği web wallet olan [ZeeJS Wallet](http://ziesha.network/zeejs/) kullanacağız.
## Güncellemeler ve duyurular [Discord](https://discord.gg/b2evaGWb)'dan duyurulacak #Pelmeni-Testnet duyuru kanalını takipte kalın.
## Resmi hesapları takip etmeyi unutmayın [Twitter](https://twitter.com/ZieshaNetwork) | [Discord](https://discord.gg/b2evaGWb) | [Telegram](https://t.me/ZieshaNetworkOfficial)
## Role selection kanalından PelmeniTestnet rolü almayı unutmayın.
## Her türlü sorularınız ve yardım ihtiyacınız için yine discordda yazabilirsiniz

# Node kurulumu ile başlayalım

## Sunucunun güncel olduğundan emin olun
```
sudo apt-get update -y && sudo apt-get upgrade -y
```
## Gerekli kütüphaneleri kurun
```
sudo apt install -y build-essential libssl-dev cmake screen git htop
```
## Discordidnizi kaydedin
```
DISCORDID="discord#1234"
```
```
echo "export DISCORDID="${DISCORDID}"" >> $HOME/.bash_profile
```


## Rustup'u yükleyin
* komutu girdikten sonra çıkan soruya 1 yazıp enter
```

curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
## Bazuka kurulumu
```
git clone https://github.com/ziesha-network/bazuka
```
```
source "$HOME/.cargo/env"
```
```
cd bazuka
```
```
git pull origin master
```
```
cargo update
```
```
cargo install --path .
```
## İnitalize
* Daha inceki testnetteki cüzdanınızın mnemoniclerini girmeyi unutmayın eğer ilk defa katılıyorsanız `--mnemonic` falagını silerek  bu komutu kullanın ve yeni cüzdan oluşturun
* Önemli; 12 kelimenizi saklamayı unutmayın.
* Eğer nodeyi başka bir sunucuya taşımak isterseniz kurulum adımları aynı kalacak şekilde sadece aşağıdaki komutta menmonicleri kullanmanız yeterli
```
bazuka init --network pelmeni --external $(wget -qO- eth0.me):8765 --bootstrap 65.108.193.133:8765 --bootstrap 213.14.138.127:8765 --mnemonic "12 kelimeyi buraya yazın"
```
## Servisi kurun ve nodu başlatın

```
tee /etc/systemd/system/bazuka.service > /dev/null <<EOF
[Unit]
Description=Bazuka
After=network.target
[Service]
User=root
ExecStart=/root/.cargo/bin/bazuka node start --discord-handle "$(DISCORDID)"
Restart=on-failure
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```
```
systemctl daemon-reload
```
```
systemctl enable bazuka
```
```
systemctl start bazuka
```
## Nodu yeniden başlatmak için
```
systemctl restart bazuka
```

## Loglara bakmak için
```
journalctl -u bazuka -fo cat
```
## Logların akışı bu şekilde olmalı

![image](https://user-images.githubusercontent.com/108215275/215479892-b01eae5b-bf2c-4e0e-a9c1-f92dfcb33aec.png)



## Node durumuna bakmak için
```
bazuka node status
```
## Çıktısı bu şekilde
![image](https://user-images.githubusercontent.com/108215275/215480078-a29351de-ed62-4edd-b6bb-e9edb20bb7b2.png)

## Nodu kurdunuz network ve version doğruysa (görseldeki gibi) height artması için biraz beklemeniz gerekiyor.
## Nodunuz ağ ile senkronize olduğunda [Explorer](http://65.108.193.133:8000/nodes)'da ip adresinizin görünmesi gerekiyor. Eğer ip adresinizi göremiyorsanız öncelikle nodunuzu kontrol edin, node sorunsuz çalışıyorsa ve explorerda görünmüyorsanız muhtemelen portlarınız kapalı olabilir. açmak için aşşağıdaki komutları kullanın.
```
sudo ufw enable
sudo firewall-cmd --zone=public --add-port=8765/tcp --permanent
sudo ufw allow 8765
sudo ufw allow 8765/tcp
sudo ufw allow 8766
sudo ufw allow 8766/tcp
sudo ufw allow 22
sudo ufw allow 22/tcp
sudo ufw enable
sudo ufw status
```
## ZeeJS Wallet'ı kullanın 
* http://ziesha.network/zeejs/ adresine gidin gidin 12 kelimenizi import edin cüzdan kullanıma hazır. 
* Buradaki adres MPN adresiniz havuza katılmak için bunu kullanacağız.
* Eğer bağlanamıyorsanız farklı tarayıcı deneyin (Operada kullanıyorum)
* https değil http ile bağlanın. Tarayıcınızın güvenlik ayarları otomatik https ile bağlanmaya çalışıyor olabilir, bu ayarı devre dışı bırakarak deneyin.

# Madencilik havuzuna katılmak için sıradaki adımlara geçebilirsiniz

## Uzi-Miner'i başlatmak için havuz sahibinden sizin cüzdanınıza tanımlı bir miner token almanız gerekiyor. Bunun için discordda mining pools odasına gidip sabit mesajlarda aktif olan havuzlardan birini seçin ([Leaderboard](http://65.108.193.133:8000/)). Ardından havuz sahibinin websitesine gidin `z` ile başlayan adresinizi girdikten sonra bir komut verecek bu komutu havuza bağlanarak madencilik yapmak için kullanacağız. 

* `uziminer=` karşısına " " işaretleri arasında havuz sitesinden aldığınız komutu girin.
```
uziminer="havuzkomutu"
```
```
echo "export uziminer="${uziminer}"" >> $HOME/.bash_profile
```

## Uzi-Miner kurulumu
```
git clone https://github.com/ziesha-network/uzi-miner
```
```
cd uzi-miner
```
```
cargo update
```
```
cargo install --path .
```





## Uzi-Miner için servis kurun ve başlatın
```

tee /etc/systemd/system/uzi.service > /dev/null <<EOF
[Unit]
Description=Uzi-Miner
After=network.target
[Service]
User=root
ExecStart=/root/.cargo/bin/$uziminer
Restart=on-failure
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```
```
systemctl daemon-reload
```
```
systemctl enable uzi
```
```
systemctl start uzi
```
## Loglara bakmak için
```
journalctl -u uzi -fo cat
```
## Log akışı buna benzerse çalışıyor
![image](https://user-images.githubusercontent.com/108215275/215481744-1fbbe8b4-c1aa-4401-8d05-a2f8a260243f.png)






























