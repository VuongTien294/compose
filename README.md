Service :
- registry-service : 8761 : đăng kí cho các service

- gateway-service : 8000 : cổng vào
 + Điều hướng đường dẫn
 + Validate token và gán thông tin của token vào header

- order-service : 8281
 + Cho người dùng order sản phẩm

- user-service : 8282
 + Cho người dùng đăng kí, đăng nhập

- product-service : 8283
 + Tạo mới và xem sản phẩm

- inventory-service : 8284
 + Kiểm tra xem sản phẩm còn trong kho không

- notification-service : 8285
 + Gửi thông báo cho khách hàng

- akhq : 8080

- pgAdmin : 16543

-kafka :
 zookeeper : 2180
 broker 1 : 9092
 broker 2 : 9093

-postgres : 5432 (5432 - 5435)

- order-service, inventory-service, notification-service sẽ giao tiếp với nhau (Synchronous hoặc Asynchronous)

- Khi người dùng đặt hàng thành công, order-service sẽ gửi mail cho user và bắn notify lên firebase

*Build Docker Image
1.Tạp file build jar
- mvn clean
- mvn package
2.Tạo 1 docker image dưới local : docker image build -t imageName:version .(VD : docker image build -t web:latest .)
 VD : docker image build -t vuongtien294/order-service:latest .
3.Xem image đó có id là gì
- docker images
4.Push image đó lên docker hub :
- docker tag <image-id> <username>/<repository-name>:<tag> (với image-id là id của image dưới local mà t muốn push)
 VD : docker tag e205091821cf vuongtien294/order-service:latest
- docker login
- docker push <username>/<repository-name>:<tag>
 VD : docker push vuongtien294/order-service:latest

*Xóa image
1.Xem container chứa image đó có đang chạy ko ?Nếu không mới xóa được
 -Xem tất cả các container đang chạy : docker ps -a
2.docker rmi idImage (Chỉ cần 3 chữ đầu của id image là docker tự hiểu)
 VD : docker rmi 8f5

*Xem container đang chạy ntn
 docker exec -it mycontainer sh (mycontainer truyền id vào)

*Xem log của container đó
 docker logs -f -n 100 mycontainer (nên dùng docker logs -f mycontainer thôi)

*Xem casc network cuar cac container
 docker network ls

*Xem cac container dang join network do
 docker network inspect network_name

*Xem cac container dnag chay
 docker container ls -a


Linux :

- "5433:5432" : 5433 là cổng bên ngoài và 5432 là cổng bên trong. Các image làm việc vs nhau qua cổng bên trong docker

- Xóa folder : rm -r direction

- Xóa file : rm tênFile

- Xem chủ sở hữu của folder : ls -l tenthumuc

* Tạo thư mục : mkdir tenthumuc (hoặc đừng dẫn thư mục đã tồn tại)
- Tạo thư mục cùng cả thư mục cha : mkdir -p path_name (dùng khi thư mục cha chưa tồn tại)

*Xem file bằng đầu ra chuẩn : cat tenFile

*Copy 1 file sang thư mục khác : cp tenFile Path_name
VD : cp img1.jpg /home/username/Pictures -> tạo 1 bản sao của file img1.jpg vào thư mục đằng sau

*Di chuyển tệp : mv path_name path_target
VD : mv /a /home/username/Documents

*Tạo 1 tệp : touch path_name
VD : touch /home/username/Documents/index.html

Cách setUp nginx :
1.Tải nginx :
sudo apt update
sudo apt install nginx
2.Lệnh để cho phép xem các giao thức có thể tương tác vs server thông qua tường lửa
-sudo ufw app list
3.Kích hoạt UFW (tường lửa)
-sudo ufw status
-sudo ufw enable (sau khi chuyển sẽ thành active)
3.Cho phép HTTP và HTTPS tương tác được vs server
sudo ufw allow 'Nginx HTTP'
sudo ufw allow 'Nginx HTTPS'
4.Kiểm tra server Nginx đã lên chưa
-systemctl status nginx
5.Truy cập thử trên trình duyệt
-curl -4 icanhazip.com : xem địa chỉ ip public của máy tính mình
-Truy cập : http://diachiippubliccuamay hoặc http://tenmiendadangki
6.Các lệnh hay dùng vs Nginx:
- Dừng : sudo systemctl stop nginx
- Chạy : sudo systemctl start nginx
- Khởi động lại : sudo systemctl restart nginx
- Khi thay đổi file cấu hình Nginx : sudo systemctl reload nginx
7.Cấu hình
-Tạo 1 file cấu hình sau đó viết cấu hình vào : sudo nano /etc/nginx/sites-enabled/myproject.conf (backend 1 file riêng, fe 1 file riêng tùy vào mình muốn cấu hình sub domain ra sao)
server {
  server_name projectcuatien.click;
    location / {
    proxy_pass http://localhost:8000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
  }
}
8.Reload lại
9.Cấu hình SSL
-sudo snap install certbot --classic
-sudo certbot --nginx -d projectcuatien.click
-Đặt lịch cứ 90 ngày sẽ tạo 1 SSL mới : sudo certbot renew --dry-run
10.Xem logs lỗi của nginx :
-sudo tail -f /var/log/nginx/error.log


sudo passwd
su
passwd root
vi /etc/ssh/sshd_config
PermitRootLogin yes
PasswordAuthentication yes
service sshd restart


