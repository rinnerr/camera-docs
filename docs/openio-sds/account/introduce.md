•	Account service lưu trữ thông tin liên quan đến account như danh sách container,
số lượng objects và số byte bị chiếm bởi tất cả các objects trong cluster. 
Sau một thao tác trên một container (PUT, DELETE), các events được tạo và sử dụng bởi account service để cập nhật thông tin account không đồng bộ.  


![](https://github.com/rinnerr/camera-docs/blob/master/docs/images/account-management.png)

• Account service sử dụng Redis để lưu trữ thông tin account. Với 1 cluster có 3 storage nodes, trên mỗi storage node đều có account service và Redis.
Và trong 3 Redis, sẽ có 1 con là master của 2 Redis còn lại. Redissentinel có nhiệm vụ trả về cho Account service thông tin Redis nào là master (master name)
và account service sẽ connect đến Redis master để update information.
