# Lab DNS Master - Slave

- DNS master-slave mục tiêu sử dụng 2 server dùng để backup dữ liệu cũng như cân bằng tải cho máy master.
 <ul>
  <li>Master là máy chủ chính lưu trữ cũng như thiết lập những file cấu hình, tuy nhiên khi số lượng file zone cũng như truy vấn nhiều đến mức nhất định mà  
  master không thể tải nổi nữa thì slave sẽ được lựa chọn dùng để đỡ bớt một phần cho master.</li>
  <li>Slave là máy chủ không cần lưu trữ các file cấu hình, nhưng nó có thể ánh xạ tất cả các file cấu hình từ server master để phân giải.</li>
 </ul>

 - Sau đây là bài lab cấu hình DNS master-slave :
 
 - Trên cả 2 server master và slave chúng ta đều phải cài đặt `Bind9` :
 
 ```sh
 sudo apt-get install bind9 -y
 ````
 
### Về thiết lập trên Master có thể xem tại [đây](https://github.com/hocchudong/ghichep-DNS/blob/master/docs/lab-dns.md) .
 
### Thiết lập Slave :

- Cấu hình file `named.conf.local` :

```sh
vi /etc/bind/named.conf.local
```

- Chỉnh sửa lại file cấu hình như sau :

```sh
//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";

zone "datpt.com" {
        type slave; // để là slave vì đây là máy chủ slave.
        file "/etc/bind/db.datpt.com"; // Đường dẫn file cấu hình thuận bên server Master
        masters {10.10.20.10; }; // Địa chỉ máy chủ Master.
};

zone "20.10.10.in-addr.arpa" {
        type slave; 
        file "/etc/bind/db.datpt.reverse";  // Đường dẫn file cấu hình nghịch bên server Master.
        masters {10.10.20.10; };  // Địa chỉ máy chủ master.
};

```

- Restart lại cấu hình dịch vụ :

```sh
service bind9 restart
```

- Đổi cấu hình trong `/etc/resolv.conf` :

```sh
nameserver 127.0.0.1
```

- Thực hiện `nslookup` để kiểm tra :

```sh
nslookup
```

![master-slave](/images/master-slave.png)

