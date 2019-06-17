Conscience có 2 chức năng:  

   • Phát hiện ra những services sẵn sàng (các services này được theo dõi bằng conscience-agent service) trong namespace và làm thế nào để liên hện với các services đó.  
   • Load Balancing: conscien thực hiện thuật toán để đánh điểm các services sẵn sàng trong namespace (điểm từ 0-100). Điểm 0 cho biết service đó trong namespace không được dùng và service, điểm càng cao cho biết service  đó có thể dùng và performance ở mức tốt.  
 ![](https://github.com/rinnerr/camera-docs/blob/master/docs/images/conscience.png)  
Conscience agent: monitor local các services trên 1 node và các services này registration trong Conscience  
