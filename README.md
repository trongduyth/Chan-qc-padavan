# Chan-qc-padavan
[<Cập nhật> Phiên bản mới, hỗ trợ WhiteList, tối ưu truy xuất hơn]
###### Truy cập Advanced Settings - Customization - Scripts - Run After Router Started
Thêm vào đoạn code vào dưới cùng
```
### Check network is ready
while true; do sleep 15; ping -c1 8.8.8.8 > /dev/null && break; done

### Create Adblock List
echo "https://adaway.org/hosts.txt" >> /tmp/adblock.list
echo "https://raw.githubusercontent.com/bigdargon/hostsVN/master/hosts" >> /tmp/adblock.list

### Create WhiteList
#echo "googleadservices.com" >> /tmp/hosts.whitelist

### Get AdBlock List
wget -qO- `cat /tmp/adblock.list` | sed 's/127.0.0.1/0.0.0.0/g' | grep -w ^0.0.0.0 | sed $'s/\r$//' | sed '/localhost/d' | sort -u > /tmp/hosts.downloaded
cat /tmp/hosts.whitelist | sed $'s/\r$//' | grep -vf - /tmp/hosts.downloaded > /tmp/hosts.blocked

### Remove downloaded list
rm -rf /tmp/adblock.list
rm -rf /tmp/hosts.downloaded

### Restart dnsmasq to update new hosts file
killall -SIGHUP dnsmasq
```

Nếu muốn thêm danh sách bỏ qua (whitelist), hãy xoá dấu '#' ở đầu dòng mục ### Create WhiteList

Ví dụ: muốn cho phép googleadservices.com và zing.vn, ta sửa như sau

```
### Create WhiteList
echo "googleadservices.com" >> /tmp/hosts.whitelist
echo "zing.vn" >> /tmp/hosts.whitelist
```

###### Truy cập Advanced Settings - LAN - DHCP Server - Custom Configuration File "dnsmasq.conf" 
Thêm vào đoạn code vào dưới cùng
```
address=/0.0.0.0/0.0.0.0
addn-hosts=/tmp/hosts.blocked
```

[Phiên bản cũ]
###### Truy cập Advanced Settings - Customization - Scripts - Run After Firewall Rules Restarted
Thêm vào đoạn code vào dưới cùng
```
sleep 30

wget --continue "https://adaway.org/hosts.txt" -O /tmp/hosts1
wget --continue "https://raw.githubusercontent.com/bigdargon/hostsVN/master/hosts" -O /tmp/hosts2

killall -SIGHUP dnsmasq
```

###### Truy cập Advanced Settings - LAN - DHCP Server - Custom Configuration File "dnsmasq.conf" 
Thêm vào đoạn code vào dưới cùng
```
addn-hosts=/tmp/hosts1
addn-hosts=/tmp/hosts2
```
