# Tìm hiểu về DNS

========
# Mục Lục:
========

[1. DNS là gì?](#dns-la-gi)

[2. Chức năng của DNS.](#chuc-nang)

[3. Nguyên tắc làm việc.](#nguyen-tac-lam-viecc)

[4. Cách sử dụng DNS.](#cach-su-dung)

[5. Kiến trúc DNS.](#kien-truc)</br>
  [5.1. Không gian tên miền (Domain name space).](#khong-gian-tien-mien)</br>
  [5.2. Tên miền (Domain name)](#ten-mien)</br>
  [5.3. Cú pháp tên miền.](#cu-phap)</br>
  [5.4. Máy chủ tên miền (Name servers).](#may-chu)</br>
  [5.5. Cách phân giải địa chỉ DNS.](#cach-phan-giai)</br>
  [5.6. Cấu trúc gói tin DNS.](#cau-truc-goi-tin)</br>


<a name="dns-la-gi"></a>
## 1. DNS là gì?

- DNS (viết tắt trong tiếng Anh của Domain Name System - Hệ thống tên miền) là một hệ thống cho
 phép thiết lập tương ứng giữa địa chỉ IP và tên miền trên Internet. 
 
- Hệ thống tên miền (DNS) về căn bản là một hệ thống giúp cho việc chuyển đổi các tên miền mà con người dễ ghi 
nhớ (dạng kí tự, ví dụ www.example.com) sang địa chỉ IP vật lý (dạng số, ví dụ 123.11.5.19) tương ứng của tên miền 
đó. DNS giúp liên kết với các trang thiết bị mạng cho các mục đích định vị và địa chỉ hóa các thiết bị trên Internet.

- Hệ thống tên miền phân phối trách nhiệm gán tên miền và lập bản đồ những tên tới địa chỉ IP bằng cách định rõ những máy 
chủ có thẩm quyền cho mỗi tên miền. Những máy chủ có tên thẩm quyền được phân công chịu trách nhiệm đối với tên miền riêng của 
họ, và lần lượt có thể chỉ định tên máy chủ khác độc quyền của họ cho các tên miền phụ. Kỹ thuật này đã thực hiện các cơ chế phân phối 
DNS, chịu đựng lỗi, và giúp tránh sự cần thiết cho một trung tâm đơn lẻ để đăng kí được tư vấn và liên tục cập nhật.

- Nhìn chung, Hệ thống tên miền cũng lưu trữ các loại thông tin khác, chẳng hạn như danh sách các máy chủ email mà chấp nhận thư điện 
tử cho một tên miền Internet. Bằng cách cung cấp cho một thế giới rộng lớn, phân phối từ khóa – cơ sở của dịch vụ đổi hướng, Hệ thống 
tên miền là một thành phần thiết yếu cho các chức năng của Internet. Các định dạng khác như các thẻ RFID, mã số UPC, kí tự Quốc tế trong 
địa chỉ email và tên máy chủ, và một loạt các định dạng khác có thể có khả năng sử dụng DNS.

<a name="chuc-nang"></a>
## 2. Chức năng của DNS.

- Mỗi website có một tên (là tên miền hay đường dẫn URL: Uniform Resource Locator) và một địa chỉ IP. Địa chỉ IP gồm 4 nhóm số cách nhau bằng dấu chấm(IPv4). 
Khi mở một trình duyệt Web và nhập tên website, trình duyệt sẽ đến thẳng website mà không cần phải thông qua việc nhập địa chỉ IP của trang web. Quá trình "dịch" tên miền 
thành địa chỉ IP để cho trình duyệt hiểu và truy cập được vào website là công việc của một DNS server. Các DNS trợ giúp qua lại với nhau để dịch địa chỉ "IP" thành "tên" và ngược 
lại. Người sử dụng chỉ cần nhớ "tên", không cần phải nhớ địa chỉ IP (địa chỉ IP là những con số rất khó nhớ)

<a name="nguyen-tac-lam-viecc"></a>
## 3. Nguyên tắc làm việc của DNS.

![dns-diagram](/images/dns-diagram.png)

- Mỗi nhà cung cấp dịch vụ vận hành và duy trì DNS server riêng của mình, gồm các máy bên trong phần riêng của mỗi nhà cung cấp dịch vụ đó trong Internet. Tức là, nếu một trình duyệt 
tìm kiếm địa chỉ của một website thì DNS server phân giải tên website này phải là DNS server của chính tổ chức quản lý website đó chứ không phải là của một tổ chức (nhà cung cấp dịch vụ) nào khác.

- INTERNIC (Internet Network Information Center) chịu trách nhiệm theo dõi các tên miền và các DNS server tương ứng. INTERNIC là một tổ chức được thành lập bởi NFS (National Science Foundation), 
AT&T và Network Solution, chịu trách nhiệm đăng ký các tên miền của Internet. INTERNIC chỉ có nhiệm vụ quản lý tất cả các DNS server trên Internet chứ không có nhiệm vụ phân giải tên cho từng địa chỉ.

- DNS có khả năng truy vấn các DNS server khác để có được 1 cái tên đã được phân giải. DNS server của mỗi tên miền thường có hai việc khác biệt. Thứ nhất, chịu trách nhiệm phân giải tên từ các máy bên 
trong miền về các địa chỉ Internet, cả bên trong lẫn bên ngoài miền nó quản lí. Thứ hai, chúng trả lời các DNS server bên ngoài đang cố gắng phân giải những cái tên bên trong miền nó quản lí.

- DNS server có khả năng ghi nhớ lại những tên vừa phân giải. Để dùng cho những yêu cầu phân giải lần sau. Số lượng những tên phân giải được lưu lại tùy thuộc vào quy mô của từng DNS.

<a name="cach-su-dung"></a>
## 4. Cách sử dụng DNS.

- Do các DNS có tốc độ biên dịch khác nhau, có thể nhanh hoặc có thể chậm, do đó người sử dụng có thể chọn DNS server để sử dụng cho riêng mình. Có các cách chọn lựa cho người sử dụng. Sử dụng DNS mặc 
định của nhà cung cấp dịch vụ (Internet), trường hợp này người sử dụng không cần điền địa chỉ DNS vào network connections trong máy của mình. Sử dụng DNS server khác (miễn phí hoặc trả phí) thì phải điền địa 
chỉ DNS server vào network connections. Địa chỉ DNS server cũng là 4 nhóm số cách nhau bởi các dấu chấm.

<a name="kien-truc"></a>
## 5. Kiến trúc DNS.

<a name="khong-gian-tien-mien"></a>
### 5.1. Không gian tên miền (Domain name space).

![domain-name-space](/images/domain-name-space.png)

- Không gian tên miền là một kiến trúc dạng cây (hình), có chứa nhiều nốt (node). Mỗi nốt trên cây sẽ có một nhãn và có không hoặc nhiều resource record (RR), chúng giữ thông tin liên quan tới tên miền. Nốt 
root không có nhãn.

<a name="ten-mien"></a>
### 5.2. Tên miền (Domain name)

- Tên miền được tạo thành từ các nhãn và phân cách nhau bằng dấu chấm (.), ví dụ example.com. Tên miền còn được chia theo cấp độ như tên miền top level, tên miền cấp 1, cấp 2...

<a name="cu-phap"></a>
### 5.3. Cú pháp tên miền.

- Hệ thống tên miền tính theo hướng từ phải sang trái. Ví dụ www.examplle.com thì nhãn example là một tên miền con của tên miền com, và www là tên miền con của tên miền example.com. Cây 
cấu trúc này có thể có tới 127 cấp.

<a name="may-chu"></a>
### 5.4. Máy chủ tên miền (Name servers).

- Máy chủ tên miền chứa thông tin lưu trữ của Không gian tên miền. Hệ thống tên miền được vận hành bởi hệ thống dữ liệu phân tán, dạng client-server. Các nốt của hệ dữ liệu này là các máy chủ tên miền. Mỗi một tên 
miền sẽ có ít nhất một máy chủ DNS chứa thông tin của tên miền đó. Các thông tin của Máy chủ tên miền sẽ được lưu trữ trong các zone. Có hai dạng NS là là primary và secondary.

<a name="phan-giai"></a>
### 5.5. Cách phân giải địa chỉ DNS.

![dns-resolve](/images/dns-resolve.png)

- Phần client của DNS gọi là DNS resolver.
 <ul>
  <li>Truy vấn non-recursive: DNS resolver client truy vấn DNS server để tìm record của tên miền chưa trên server đó.</li>
  <li>Truy vấn recursive</li>
  <li>Truy vấn iterative</li>
 </ul>
 
- DNS query :

![dns-request](/images/dns-request.jpg)

<a name="cau-truc-goi-tin"></a>
### 5.6. Cấu trúc gói tin DNS.

- ID: Là một trường 16 bits, chứa mã nhận dạng, nó được tạo ra bởi một chương trình để thay cho truy vấn. Gói tin hồi đáp sẽ dựa vào mã nhận dạng này để hồi đáp lại. Chính vì vậy mà truy vấn và hồi đáp có thể phù hợp với nhau.
- QR: Là một trường 1 bit. Bít này sẽ được thiết lập là 0 nếu là gói tin truy vấn, được thiết lập là một nếu là gói tin hồi đáp.
- AA: Là trường 1 bit, nếu gói tin hồi đáp được thiết lập là 1, sau đó nó sẽ đi đến một server có thẫm quyền giải quyết truy vấn.
- TC: Là trường 1 bit, trường này sẽ cho biết là gói tin có bị cắt khúc ra do kích thước gói tin vượt quá băng thông cho phép hay không.\
- RD: Là trường 1 bit, trường này sẽ cho biết là truy vấn muốn server tiếp tục truy vấn một cách đệ quy.
- RA: Trường 1 bit này sẽ cho biết truy vấn đệ quy có được thực thi trên server không.
- Z: Là trường 1 bit. Đây là một trường dự trữ, và được thiết lập là 0.
- Rcode: Là trường 4 bits, gói tin hồi đáp sẽ có thể nhận các giá trị sau:
 <ul>
  <li>0: Cho biết là không có lỗi trong quá trình truy vấn.</li>
  <li>1: Cho biết định dạng gói tin bị lỗi, server không hiểu được truy vấn.</li>
  <li>2: Server bị trục trặc, không thực hiện hồi đáp được.</li>
  <li>3: Tên bị lỗi. Chỉ có server có đủ thẩm quyền mới có thể thiết lập giá trị náy.</li>
  <li>4: Không thi hành. Server không thể thực hiện chức năng này.</li>
  <li>5: Server từ chối thực thi truy vấn.</li>
 </ul>
- QDcount: Số lần truy vấn của gói tin trong một vấn đề.
- ANcount: Số lượng tài nguyên tham gia trong phần trả lời.
- NScount: Chỉ ra số lượng tài nguyên được ghi lại trong các phần có thẩm quyền của gói tin.
- ARcount: Chỉ ra số lượng tài nguyên ghi lại trong phần thêm vào của gói tin.


# Tham Khảo :

- https://vi.wikipedia.org/wiki/DNS#T.E1.BB.95ng_quan
- http://www.hwmn.org/w/Talk:DNS_Infrastructure
- http://support.limestonenetworks.com/knowledge-base/how-does-dns-operate/
    
    
