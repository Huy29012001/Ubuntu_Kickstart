# Ubuntu_Kickstart

    + HOW_TO_USE
    
    -   Tải xuống dạng ZIP, giải nén và đưa vào PATH = C:\pxesrv\*.*
    
    -   Tải ISO Ubuntu Server 2004, giải nén vào folder '2004'
    
    -   Khởi động NFS Server:
    
         C:\pxesrv\WinNFSd-2.0\winnfsd.exe -id 0 0 C:\pxesrv\files\2004
        * IP của Client và Server phải nằm cùng 1 dải IP
        * Kiểm tra "Local IP" trong NFS Server (Vì có thể nhận nhầm IP của VMWare, VBox,..)

    -   Chỉnh cấu hình IP trong PXELinux:
    
         C:\pxesrv\files\pxelinux.cfg\default
        * Chỉnh về đúng IP Server (Trùng với IP trong NFS Server)

    -   Khai báo biến cho cloud-init (Chạy Auto Install)

         C:\pxesrv\files\2004\kickstart\meta-data
        * Đặt tên cho máy Client
         C:\pxesrv\files\2004\kickstart\user-data
        * Bao gồm đặt tên, tạo user, khai báo IP tĩnh cho Client, các gói cài đặt cần thiết,...(Nên để mặc định)

    - Chạy TinyPXE
         C:\pxesrv\pxesrv.exe
        * Kiểm tra "Option54" (Vì có thể nhận nhầm IP của VMWare, VBox,..), sau đó chọn "Online"
        (Nếu có thông báo Firewall hiện ra, chọn Allow)
        
    -   Kiểm tra lại toàn bộ, bật máy Client và Set Boot Option = PXE Network và lót dép ngồi hóng :P 


    + Q & A

    -   Q: Client k nhận ra PXE Server
        A: Kiểm tra dải IP "Option54" trùng với dải IP máy Client chưa?
           Với VMWare phải dùng dải Host-Only, NAT, VMnet; không dùng được Bridge (Chắc VMWare có bug :P)
        
        Q: Boot tới initramfs báo lỗi perrmission denied
        A: Kiểm tra lại Syntax trong NFS Server bao gồm PATH và id

        Q: Boot tới initramfs báo lỗi connection refused
        A: Kiểm tra lại IP NFS Server (Có thể phải disable tạm thời các NIC không cần tới)

    + Tip
    - Nếu muốn cài mỗi máy Client 1 cấu hình khai báo khác nhau, nhìn trên Log TinyPXE, khi 1 máy đã đọc 2 file user-data và meta-data xong thì có thể sửa trực tiếp 2 file đó và tiếp tục cài cho máy tiếp theo với cấu hình khai báo khác.

    Updating... 