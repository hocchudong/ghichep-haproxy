# 3. Cơ bản về các dòng cấu hình trong file cấu hình của KeepAlived

____

# Mục lục


- [3.1 Các block cấu hình trong keepalived](#about)
- [3.2 Nội dung cấu hình trong block `global_defs`](#global_defs)
- [3.3 Nội dung cấu hình trong block `static_ipaddress static_routes`](#static_ipaddress)
- [3.4 Nội dung cấu hình trong block `vrrp_instance`](#vrrp_instance)
- [](#)
- [Các nội dung khác](#content-others)

____

# <a name="content">Nội dung</a>

- ### <a name="about">3.1 Các block cấu hình trong keepalived</a>

    - Trong cấu hình của keepalived được chia thành các block cấu hình đảm nhiệm các chức năng riêng biệt bao gồm:

        + global_defs
        + static_ipaddress, static_routes
        + vrrp_instance
        + vrrp_sync_group
        + vrrp_script
        + virtual_server
        + virtual_server_group

- ### <a name="global_defs">3.2 Nội dung cấu hình trong block `global_defs`</a>

    + Chức năng của block này là cho phép cấu hình để nhận thông báo khi có sự cố xảy ra trong hệ thống load balancer. Đây là block cấu hình tùy chọn nên không nhất thiết phải có trong file cấu hình của KeepAlived.

    + Cách khai báo cấu hình block `global_defs` như sau:

            global_defs {
                notification_email {
                    ...
                }
                notification_email_from
                ...
            }

    + Trong block chính global_defs có thể bao gồm các cấu hình như sau:

        - notification_email: Khai báo danh sách các địa chỉ email sẽ nhận được nội dung mail thông báo khi có lỗi xảy ra. Nó ứng với nội dung trong trường điền `To: ` khi bạn thực hiện gửi mail. Ví dụ:

                notification_email {
                    admin@gmail.com
                    admin1@gmail.com
                    admin2@gmail.com
                }

        hoặc 

                notification_email admin@gmail.com

        - notification_email_from: Khai báo địa chỉ thực hiện gửi mail chứa nội dung các lỗi xảy ra trong load balancers đến các mail khai báo trong `notification_email`. Nó tương ứng với nội dung trong trường `From: ` khi bạn thực hiện gửi một mail. Ví dụ:

                notification_email_from admin@exit.com

        hoặc 

                notification_email_from {
                    admin@exit.com
                    admin1@exit.com
                    admin2@exit.com
                }
    
        - smtp_server: Khai báo một địa chỉ IP, hoặc tên host dùng để remote SMTP server dùng để gửi mail thông báo. Ví dụ:

                smtp_server: 127.0.0.1

        - smtp_connect_timeout: Quy định thời gian chờ cho quá trình xử lý SMTP stream, đơn vị tính theo giây (s). Ví dụ:

                smtp_connect_timeout 30

        - lvs_id: Khai báo tên định danh cho LVS director. Ví dụ:

                lvs_id LVS01
        - enable_traps: Kích hoạt SMPT traps. Ví dụ:

                enable_traps

        - vrrp_mcast_group4: Khai báo địa chỉ IPv4 gửi multicast. Mặc định là: 224.0.0.18

        - vrrp_mcast_group6: Khai báo địa chỉ IPv6 gửi multicast. Mặc định là: ff02::12

- ### <a name="static_ipaddress">3.3 Nội dung cấu hình trong block `static_ipaddress`, `static_routes`</a>

    - Chỉ thị cho phép cấu hình địa chỉ IP tĩnh và cấu hình định tuyến tĩnh. Không bắt buộc phải có phần cấu hình này nếu các máy chủ đã có kết nối mạng.

        + static_ipaddress: Cấu hình địa chỉ IP tĩnh. Ví dụ:

                static_ipaddress {
                    192.168.1.1/24 dev eth0 scope global
                    192.168.1.3/24 dev eth0 scope global
                    ...
                }

        + static_routes: Cấu hình định tuyến tĩnh tới các mạng. Ví dụ:

                static_routes {
                    192.168.2.0/24 via 192.168.1.100 dev eth0
                    192.168.4.0/24 via 192.168.1.100 dev eth0
                    ...
                }

- ### <a name="vrrp_instance">3.4 Nội dung cấu hình trong block `vrrp_instance`</a>

    + vrrp_instance: là block cho phép cấu hình chuyển đổi địa chỉ IP (chuyển đổi các địa chỉ VIP) ứng với mỗi trường hợp của một nhóm trong vrrp_sync_group.

    + Cách khai báo chung cho block `vrrp_instance` như sau:

        vrrp_instance name {
            state MASTER|BACKUP|SLAVE
            interface eth0
            ...
        }

    + Trong block `vrrp_instance` có thể khai báo các cấu hình sau:

        
- ### <a name=""></a>


____

# <a name="content-others">Các nội dung khác</a>
