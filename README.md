<h1 align="center"> Fuel Beta-5 Ağında Kontrat Deploy Etme </h1>

## Bilgi
> Fuel ağı çok güçlü bir şekilde geliyor ve son dönemlerde developerlar çok ciddi airdroplar kazanıyor.

> Hetzner üzerinden bir sunucu açtım(2gb-4gb-80gb), belki codespace üzerindende yapılabilir.

> İşlemler uzun ve ne yazık ki yorucu yapmak isterseniz buyrun

> Bu yollara Ruesandora hocam sayesinde girdim. Kendisini https://x.com/Ruesandora0 bu hesaptan takip edebilirsiniz.

<h1 align="center"> Başlayalım </h1>

```console
# Sunucu güncelleme ile başlayalım, ikinci komutta y diyip devam edebilirsiniz
sudo apt update && sudo apt upgrade -y
sudo apt-get install git-all
sudo apt-get install curl

# Rust yükleyelim, ilk komutta 1 e basıp devam edelim
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source "$HOME/.cargo/env"
cd $home && rm -rf .fuel && rm -rf .forc && rm -rf .fuelup && rm -rf fuel-project

# Alttaki komutu girince y ile devam edelim lütfen
curl --proto '=https' --tlsv1.2 -sSf https://install.fuel.network/fuelup-init.sh | sh

# Fuelup kurmamız gerekiyor
git clone https://github.com/FuelLabs/fuelup
cd fuelup

# Cargo yükleyip, sonrasında fuelup için devam edeceğiz.
apt install cargo
cargo run --release

# Devam ediyoruz.
fuelup self update
fuelup toolchain install beta-5
fuelup default beta-5
fuelup --version

# Kontrat işlemlerine geçelim
mkdir fuel-project
cd fuel-project
forc new counter-contract

# Aşağıdaki komutu girip, içerisindekileri silelim
nano counter-contract/src/main.sw

# Yukarıdaki dosyanın içerisinde aşağıdaki komutu olduğu gibi girelim ve CTRL X+Y enter ile çıkalım o ekrandan
contract;
 
storage {
    counter: u64 = 0,
}
 
abi Counter {
    #[storage(read, write)]
    fn increment();
 
    #[storage(read)]
    fn count() -> u64;
}
 
impl Counter for Contract {
    #[storage(read)]
    fn count() -> u64 {
        storage.counter.read()
    }
 
    #[storage(read, write)]
    fn increment() {
        let incremented = storage.counter.read() + 1;
        storage.counter.write(incremented);
    }
}

# Kontrat sekmesine ilerliyoruz
cd counter-contract
forc build

# Cüzdanımızı ekliyoruz
forc-wallet import
forc wallet account new
forc wallet accounts

# Cüzdanınıza [Faucet](https://faucet-beta-5.fuel.network/) alın
# Ve kontrat deploy edin
forc deploy --testnet

# Bir süre sonra https://fuellabs.github.io/block-explorer-v2/beta-5/#/ buradan cüzdan adresinizle kontrol edebilirsiniz.

```

