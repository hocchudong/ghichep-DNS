# Lab phân giải địa chỉ tên miền sử dụng dịch vụ Bind9


## 1. Mô hình thực hiện.

## 2. Thực hiện.

- Trên máy chủ DNS thực hiện cài đặt Bind9:

```sh
sudo apt-get install bind9
```

- Sau khi cài đặt xong tất cả các cấu hình được lưu trữ tại :

```sh
/etc/bind
```

- Để cấu hình phân giải tên miền chúng ta cần cấu hình file zone, file phân giải thuận và file phân giải nghịch:

### Thêm một zone :

- Để thêm mới một zone chúng ta thêm vào file `/etc/bind/named.conf.local` :

```sh
vi /etc/bind/named.conf.local
```

Ví dụ ở đây tôi muốn phân giải địa chỉ `10.10.20.20` thành tên miền `datpt.com` trên máy chủ DNS server có địa chỉ `10.10.20.10` sẽ thêm vào zone mới như sau :

```sh
//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";

zone "datpt.com" {
        type master;
        file "/etc/bind/db.datpt.com";
};

zone "20.10.10.in-addr.arpa" {
        type master;
        file "/etc/bind/db.datpt.reverse";
};
~

```

- zone "datpt.com" là zone thuận với file cấu hình thuận là `/etc/bind/db.datpt.com` (file tùy thích)

- zone "20.10.10.in-addr.arpa" là zone nghịch với file cấu hình nghịch là `"/etc/bind/db.datpt.reverse"` (file tùy thích)

- Sau khi đã cấu hình xong file zone chúng ta tiến hành cấu hình file cấu hình thuận , copy file config mẫu vào file cấu hình thuận :

```sh
cp /etc/bind/db.empty /etc/bind/db.datpt.com
```

- Chỉnh sửa lại file cấu hình thuận với các cấu hình như sau :

```sh

; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     ns.datpt.com. root.datpt.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns.datpt.com.
@       IN      A       10.10.20.10
@       IN      AAAA    ::1
ns      IN      A       10.10.20.10
www     IN      A       10.10.20.20

```

- Với file cấu hình trên chúng ta có thể hiểu các thông số như sau :
 <ul>
  <li>ns.datpt.com. : Đây là địa chỉ mà dns phân giải cho DNS server</li>
  <li>root.datpt.con. : Địa chỉ email</li>
  <li>@   IN  NS  ns.datpt.com. : Đây là tên miền phân giải cho DNS server</li>
  <li>@       IN      A       10.10.20.10 : Địa chỉ IP của DNS server</li>
  <li>ns      IN      A       10.10.20.10 : tên miền phân cấp đằng trước datpt.com trỏ về địa chỉ IP</li>
  <li>www     IN      A       10.10.20.20 : IP sẽ được trỏ trực tiếp đến datpt.com</li>
 </ul>

- Tiếp theo chúng ta copy file cấu hình mẫu cho file phân giải nghịch và cấu hình :

```sh
cp /etc/bind/db.empty /etc/bind/db.datpt.reverse
```

- Mở file cấu hình nghịch với trình soạn thảo vi :

```sh
vi /etc/bind/db.datpt.reverse
```

- Chỉnh sửa file cấu hình nghịch với nội dung như sau :

```sh
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     ns.datpt.com. root.datpt.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns.
10      IN      PTR     ns.datpt.com.
20      IN      PTR     www.datpt.com.

```

- Trong đó :
 <ul>
  <li>10      IN      PTR     ns.datpt.com. : là địa chỉ được gán với ns.datpt.com</li>
  <li>20      IN      PTR     www.datpt.com. : là địa chỉ được gán với datpt.com</li>
 </ul>
 
- Lưu lại file cấu hình .

- Sửa file `/etc/resolv.conf`

```sh
vi /etc/resolv.conf
```

- Với thông số :

```sh
nameserver 127.0.0.1
```

- Restart lại máy chủ :

```sh
service bind9 restart
```

- Kiểm tra lại cấu hình :

```sh
nslookup
```

- Trên máy chủ được phân giải `10.10.20.20` chúng ta đăng nhập vào và mở file  `/etc/resolv.conf` sửa lại :

```sh
nameserver 10.10.20.10 // là địa chỉ của DNS server.
```


![nslookup](/images/nslookup.png)

### Test trên client windows:

- Trỏ địa chỉ phân giải của client về DNS server vừa cấu hình :

![client-dns1](/images/client-dns1.png)

- Mở trình duyệt lên và gõ vào địa chỉ `datpt.com`

![client-dns2](/images/client-dns2.png)