**Directory Services:** gồm Meta0, Meta1 and Meta2 services, chịu trách nhiệm xử lý các directory requests và storing metadata  

![](https://github.com/rinnerr/camera-docs/blob/master/docs/images/directory-services.png)  
•	Containers và Objects được lưu trữ trong một thư mục phân tán ba cấp liên tục (Meta-0, Meta-1, Meta-2). OpenIO SDS có thể lưu trữ hàng trăm dịch vụ cho mỗi trăm triệu container. Thư mục có dạng hash table, ánh xạ containers’ UUIDs vào các services của họ. 

![](https://github.com/rinnerr/camera-docs/blob/master/docs/images/Distributed%20Three-Level%20Directory.png)  
•	Mỗi thư mục có một bản sao được đảo ngược; tức là, mỗi mục đều biết parents của nó. Một container nhận biết account của nó và một chunk  nhận biết về object và ID của container mà nó thuộc về. Điều này cho phép rebuild lại một thư mục với một lần thu thập dữ liệu đơn giản các mục có trên các storage nodes. Ngay cả khi một container bị mất, vẫn có thể tái cấu trúc nó trực tiếp từ dữ liệu. Thông thường, các container được sao chép và xây dựng lại mà không cần phải thu thập thông tin thư mục ngược.  
•	Storage nodes cũng được thu thập thông tin định kỳ để kích hoạt các hành động trên mỗi mục (ví dụ: kiểm tra tính toàn vẹn hoặc khả năng truy cập chunk).  
•	Tất cả các tác vụ quản trị này đều có thể truy cập thông qua API REST, do đó rất dễ thực hiện các hành động bảo trì theo yêu cầu  

![](https://github.com/rinnerr/camera-docs/blob/master/docs/images/Integrity%20Loop.png)
