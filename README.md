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
    - ether2 `to` ether2-Hotspot1 -|
    - ether3 `to` ether3-Hotspot2  |-> (as LAN/Access Point/Hotspot)
    - ether4 `to` ether4-Hotspot3 -|
- [Optional but recommended]: set `bridge` or `group` to make it simple, with example ports of device above here i want to make `bridge` for it
    Here as example list name of my `bridge`, you can modify it
    - bridge-WAN (ports: ether1)
    - bridge-LAN (ports: ether2, ether3, ether4)
    Now open `Bridge > Bridge > [+]` and added bridge with name above (or you can modify it), and next added port to group you have created with schema above in `Bridge > Ports > [+]` and select ether1 as member of bridge-WAN and ether2, ether3, ether4 as member of bridge-LAN like above or you can modify it 
- Request IP from ISP and for ISP ports (or ether1-ISP for schema above or you can modify it), open `IP > DHCM (Dynamic Host Configuration Protocol) > [+]` and select `interface` from ISP (or ether1-ISP/bridge-WAN for multiple interface) and make sure status is `bound`. Now set DNS in `IP > DNS` with google DNS like below
    - 8.8.8.8
    - 8.8.4.4
    Now check with RouterOS terminal and run command below to make sure internet connection is success
    ```bash
    $ ping google.com
    ```

