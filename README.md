# Sui Devnet Türkçe Kurulum Rehberi
![image](https://user-images.githubusercontent.com/102043225/179367282-e34379a6-3f7c-4273-887e-86251793610c.png)

## Sui 0.6.2 Güncellemesi
Buradan güncelemeyi yapabilirsiniz; https://github.com/koltigin/Sui-Devnet-Turkce-Kurulum-Rehberi/blob/main/sui-0.6.2-guncellemesi.md

## Sistemi Güncelleme
```shell
sudo apt update && sudo apt upgrade -y
```

## Gerekli Kütüphanelerin Kurulması
```shell
sudo apt install make clang pkg-config libssl-dev build-essential git jq ncdu bsdmainutils htop -y < "/dev/null"
```

## Paketlerin Yüklenmesi
```shell
sudo wget -qO /usr/local/bin/yq https://github.com/mikefarah/yq/releases/download/v4.23.1/yq_linux_amd64 && chmod +x /usr/local/bin/yq
sudo apt-get install jq -y
```
## Docker Kurulumu
```shell
sudo apt-get install ca-certificates curl gnupg lsb-release -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io -y
```

## Docker Compose Yüklenmesi
```shell
docker_compose_version=$(wget -qO- https://api.github.com/repos/docker/compose/releases/latest | jq -r ".tag_name")
sudo wget -O /usr/bin/docker-compose "https://github.com/docker/compose/releases/download/${docker_compose_version}/docker-compose-`uname -s`-`uname -m`"
sudo chmod +x /usr/bin/docker-compose
```

## Docker Compose Yapılandırılması
```shell
cd $HOME && rm -rf sui
mkdir sui && cd sui
wget -qO docker-compose.yaml https://raw.githubusercontent.com/MystenLabs/sui/main/docker/fullnode/docker-compose.yaml
wget -qO fullnode-template.yaml https://github.com/MystenLabs/sui/raw/main/crates/sui-config/data/fullnode-template.yaml
wget -qO genesis.blob https://github.com/MystenLabs/sui-genesis/raw/main/devnet/genesis.blob
sed -i 's/127.0.0.1/0.0.0.0/' fullnode-template.yaml
docker-compose down --volumes
```

## Uygulamayı Başlatma
```shell
docker-compose up -d
```

## Logları Görüntüleme
```shell
docker logs -f sui-fullnode-1 --tail 50
```

## Explorer 
Node'u control etmek için bu [adrese](https://node.sui.zvalid.com/) giderek ip adresinizi giriniz.

## Faydalı Komutlar

### Loglar:
```shell
journalctl -u suid -f -o cat
```

### Node Silmek
```shell
sudo systemctl stop suid
sudo systemctl disable suid
sudo rm -rf ~/sui /var/sui/
sudo rm /etc/systemd/system/suid.service
```

### Node Durumu Kontrol
```shell
service docker status
```

### Node'u Yeniden Başlatma
```shell
sudo systemctl restart suid
```

### Node'u Durdurma
```shell
sudo systemctl stop suid
```
