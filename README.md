# Tools
- `winbox`
- `mikhmon`
- `RJ45`
- `ISP`

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

