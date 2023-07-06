# Tools
- `winbox`
- `mikhmon`
- `RJ45`
- `ISP`
- `Mikrotik` `

# Example Mikrotik Device
- ports
    
    - ether1 (for ISP/WAN (Web Area Network))
    - ether2 -|
    - ether3  |-> (as LAN/for Access Point) 
    - ether4 -|

# Steps
- Open `winbox` and connect to mikrotik RouterOS 
    <!-- TODO: add image/video here -->

- Connect your ISP to `ether1` and connect other `RJ45` wired to your laptop/pc

- First, reset or remove mikrotik with default configuration in `System > Reset Configuration > "Do Not Backup" > Reset Configuration ` 

- Added new user with password, so that no just anyone can enter at will, as example below :
    open `System > Users` and added new user below (you can modify it like you want) 
    - username : `Example`
    - password : `Example`
    - group    : `full`
    and disable default user (`admin`)

- [Optional]: you can relogin to `Mikrotik` with `winbox` with the username & password you create above

- [Optional]: you can rename the default `Mikrotik` name in `System > Identity`

- [Optional but recommended]: rename each port according to its function, as example with ports above:
    - ether1 `to` ether1-ISP (to retrieve internet connection from ISP)
    - ether2 `to` ether2-Hotspot -|
    - ether3 `to` ether3-Hotspot  |-> (as LAN/Access Point/Hotspot)
    - ether4 `to` ether4-Hotspot -|

- [Optional but recommended]: set `bridge` or `group` to make it simple, with example ports of device above here i want to make `bridge` for it
    Here as example list name of my `bridge`, you can modify it
    - bridge-WAN (ports: ether1 as ISP)
    - bridge-LAN (ports: ether2, ether3, ether4 as Hotspot)
    Now open `Bridge > Bridge > [+]` and added bridge with name above (or you can modify it), and next added port to group you have created with schema above in `Bridge > Ports > [+]` and select ether1 as member of bridge-WAN and ether2, ether3, ether4 as member of bridge-LAN like above or you can modify it 

- Request IP from ISP and for ISP ports (or ether1-ISP for schema above or you can modify it), open `IP > DHCP (Dynamic Host Configuration Protocol) > [+]` and select `interface` from ISP (or ether1-ISP/bridge-WAN for multiple interface) and make sure status is `bound`. Now set DNS in `IP > DNS` with google DNS like below
    - 8.8.8.8
    - 8.8.4.4
    Now check with RouterOS terminal and run command below to make sure internet connection is success
    ```bash
    $ ping google.com
    ```
- Memberikan IP untuk `bridge-LAN` agar setiap port dalam bridge tersebut dapat mengakses internet dengan masuk ke menu `IP > Address > [+]` dengan contoh IP Address di bawah (atau anda dapat memberikan IP sesuka hati)
    - `10.10.10.1/24`

- Karena `bridge-LAN` ini nantinya akan meyebarkan koneksi internet maka dia harus punya kemampuan bagi-bagi IP agar perangkat yang terkoneksi dengan `bridge-LAN` ini dapat terkoneksi internet dengan menu `IP > DHCP Server > "DHCP Setup"` dan pilih port yang akan diberikan kemampuan untuk bagi-bagi IP dan disini saya akan menggunakan port-port yang terdaftar di `bridge-LAN`    
- Selanjutnya adalah mengatur `srcnat` untuk memberitahu `bridge-LAN` dari mana sumber/port koneksi internet dengan menu `IP > Firewall > NAT > [+]` dengan konfigurasi seperti dibawah
    - `General`  
        - chain          : `srcnat`
        - Out. Interface : `bridge-WAN`
    - `Action` 
        - Action : `masquerade`

    sampai disini seharusnya semua port yang termasuk kedalam `bridge-LAN` harusnya sudah dapat terkoneksi ke internet, tapi saat ini setiap tersambung dengan `Mikrotik` maka akan langsung terhubung ke internet sedangkan dalam kasus ini saya ini membuat sistem `Voucher Wifi` yang mengharuskan user harus login dulu lalu bisa terkoneksi ke internet, untuk itu ikuti langkah dibawah.

- Untuk mengaktifkan sistem login sebelum mendapatkan koneksi internet masuk ke menu `IP > Hotspot > "Hotspot Setup"` dan ikuti konfigurasi berikut:
    - Hotspot Interface         : pilih interface mana yang memerlukan login dulu sebelum terkoneksi internet
    - Local Address of Network  : `default` + [+] masquerade Network
    - Address Pool of Network   : `default` / anda dapat membatasi range IP yang akan dibagikan oleh interface hotspot yang dipilih
    - Select Certificate        : `default`
    - IP Address of SMTP Server : `default` / anda dapat memodifikasi sesuai keperluan
    - DNS Server                : gunakan DNS Google/Cloudflare/lainnya sesuai kebutuhan anda (`default                             : DNS Google`)
    - DNS Name                  : alaman domain untuk login agar mendapatkan koneksi internet untuk hotspot ini

    setelah melakukan konfigurasi hotspot maka setiap terkoneksi ke port/bridge terkait perlu login dulu untuk mendapatkan koneksi internet

- Mengatur waktu mikrotik agar ketika terjadi error kepada mikrotik kita dapat mentracking errornya jam berapa dan sebagainya. Caranya dengan masuk ke `System > SNTP Client` untuk link `(Primary/Secondary) SNTP Server` anda dapat mencarinya [disini](https://www.pool.ntp.org/) sesuai dengan wilayah anda dan karena saya berada di indonesia saya meakai `SNTP Server` indonesia dan selanjutnya ikuti konfigurasi berikut
    - Enable: `true`
    - Primary NTP Server: `0.id.pool.ntp.org` 
    - Secondary NTP Server: `1.id.pool.ntp.org` 


