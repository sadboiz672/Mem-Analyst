<h1 align="center">
 
  <br>
  Cài profile linux cho Volatility 3
  <br>
</h1>


## Bước 1: Install Volatility 3

- Cài đặt theo hướng dẫn từ **[Github](https://github.com/volatilityfoundation/volatility3)**
 ![image](https://user-images.githubusercontent.com/42565778/207274968-5a7213bc-1d4e-4a59-8eb6-2c47f0243984.png)
- Sau khi cài đặt thành công

![image](https://user-images.githubusercontent.com/42565778/207276297-d590d310-cca6-420c-a12e-377100194fdd.png)


## Bước 2: Tải các profile về máy

Tải các profile(nếu chưa có) [**windows**](https://downloads.volatilityfoundation.org/volatility3/symbols/windows.zip), [**linux**](https://downloads.volatilityfoundation.org/volatility3/symbols/linux.zip), [**mac**](https://downloads.volatilityfoundation.org/volatility3/symbols/mac.zip). 

Đây là các profile cơ bản, với windows và mac khá đầy đủ, Với linux phải tùy biến tự mình bổ sung, sẽ hướng dẫn chi tiết hơn bên dưới.

Bước 3: Thêm profile vào Volatility 3
- Tải các profile theo link ở trên.
- Copy hoặc chuyển vào thư mục symbols trong thư mục cài Volatility3 theo đường dẫn :
Ví dụ:          D:\volatility3\volatility3\symbols    

 ![image](https://user-images.githubusercontent.com/42565778/207276895-f417354b-e26b-40b8-a5c1-70a5e3ecd6d9.png)
 
-	Tại đây các bạn để nguyên file zip như vậy cũng được, Volatility3 vẫn sẽ đọc được hết. Hoặc có thể giải nén ra, mình tạo các thư mục và giải nén các file .zip vừa tải về vào đó cho dễ quản lý(như ảnh trên)

-	Đến với profile của linux. File profile tải về của linux còn thiếu rất nhiều, cho nên cần phải tự build thêm


## Bước 4: Kiểm tra kernel linux từ RAM và setup môi trường

- Dùng plugin banners.Banners trong Volatility3 để kiểm tra VD có file Ram tên memory.raw, dùng câu lệnh kiểm tra kernel:
 ` python .\vol.py -f .\memory.raw banners.Banners`

 ![image](https://user-images.githubusercontent.com/42565778/207277997-ab81d6ac-a2b7-436e-8fda-018d8d7c38d9.png)
 

 - Vào link [ddebs.ubuntu](http://ddebs.ubuntu.com/ubuntu/pool/main/l/linux/) để tìm và tải phiên bản nhân của Linux, sau đó tải phiên bản có đuôi amd64

![image](https://user-images.githubusercontent.com/42565778/207280394-139b8ea8-e7d6-49a7-b1b1-0f0deb31c31f.png)

 
 - Khởi tạo profile với dpkg để trích xuất file kelnel vừa tải:  `Sudo dpkg –x [tên file kernel] /[thự đựng trích xuất]` 

`sudo dpkg -x linux-image-unsigned-4.15.0-112-generic-dbgsym_4.15.0-112.113_amd64.ddeb ./`

- Trích xuất xong sẽ được folder `usr`
 
 ![image](https://user-images.githubusercontent.com/42565778/207284885-a46cb628-a5e4-4509-a65d-bb58a431659c.png)

- Vào theo đường dẫn `/usr/lib/debug/boot/` sẽ thấy file vmlinux-5.15.0-25-generic:

![image](https://user-images.githubusercontent.com/42565778/207522125-46fe72ad-781a-4421-a3d4-ee71f81b4cd1.png)


Cài Dwarf2json trên kali linux: `sudo apt install dwarf2json`
![image](https://user-images.githubusercontent.com/42565778/207522239-6e04858a-f016-46e2-aa1e-fb6257653136.png)

Tạo profile cho kernel Linux Version 5.15.0-25-genneric `* tối thiểu 4gb ram, 4 luồng`
- Dùng câu lệnh để tạo profile : `dwarf2json linux --elf [tên file kernel]> [tên file đầu ra].json`
Ví dụ tên file ở đây là vmlinux-5.15.0-25-generic thì câu lệnh như sau: 

`dwarf2json linux --elf vmlinux-5.15.0-25-generic > vmlinux-5.15.0-25-generic.json`

![image](https://user-images.githubusercontent.com/42565778/207523742-9f7fe812-cf04-49fd-86d3-82771c682928.png)


Copy hoặc chuyển file .json này vào thư mục symbols trong Volatility3

 ![image](https://user-images.githubusercontent.com/42565778/207523863-14591dd4-5716-4266-8da9-8e7baaac8c5b.png)


Tạo và thêm profile linux thành công.

![image](https://user-images.githubusercontent.com/42565778/207524591-43f18ebf-dd32-4247-bb6a-379f2f114e02.png)
 

Lần đầu chạy sau thi thêm profile sẽ mất một chút thời gian để volatility3 update, cách này khá nhanh nếu có thể dùng vol3, nhưng một số module của vol3 cho linux đang trong thời gian buiild vì thế sẽ có nhiều hạn chế. Cho nên nếu có ý định sử dụng cách build này cần cân nhắc và lưu ý.
                                                                                 
`Author: dieupx & Hoabibo`
