# note-aws-developer-associate-level

### Lambda
 - Càng tăng RAM thì CPU càng tăng (không thể tách biệt 1 trong 2, min = 128MB, max = 10GB)
 - Để cải thiện perfomance có thể
   - Tách biệt phần kết nối DB khỏi hàm(ví dụ có 10 request liên tục, nếu nhét connect DB vào trong hàm thì có mỗi lần xử lý sẽ phải connect lại, còn nếu tách ra ngoài thì chỉ cần connect 1 lần, rồi 9 lần còn lại chỉ cần xử lý logic sau đó mà k cần kết nối lại DB)
<hr/>

### SQS
 - Thời gian lưu trữ message có thể setting: 1 - 14 days. Mặc định là 4 ngày
 - Có thể chỉ định số lượng message tối đa được phép lấy trong 1 lần (max = 10)
 
 - #### messageVisibleTimeout
   Để tránh không cho nhiều consumer xử lý cùng 1 message thì có messageVisibleTimeout, có thể hiểu đây là thời gian tối đa consumer được phép xử lý message (default = 30s, minimum = 0, maximum = 12h)
 - #### delay queue
   Để set thời gian delay cho message enqueue => sử dụng delayQueue (default = 0s, minimum = 0s, maximum = 15 minutes)
 => Sự khác biệt giữa messageVisibleTimeout và delayQueue là:
   - messageVisibleTimeout sẽ bị ẩn đối với các consumer khác MỖI LẦN có 1 consumer nhận message
   - delayQueue sẽ ẩn message đối với TẤT CẢ consumer lần đầu tiên nó được đưa vào queue

 - #### Size message
   - Tối thiểu là 1 byte (1 ký tự)
   - Max size sẽ là 256KB
   - Có thể sử dụng Amazon SQS Extended Client Library for Java để tăng kích thước tối đa lên 2GB
<hr/>

### Kinesis
 - Hỗ trợ việc thu thập, phân tích dữ liệu real-time
 - Xử lý dữ liệu ở bất kỳ quy mô nào
 - Hỗ trợ thu thập dữ liệu như:
   - Âm thanh
   - Log
   - Video
   - click chuột
 - #### Amazon Kinesis Video Streams
   - Thu thập, xử lý và lưu trữ dữ liệu video từ nguồn phát để phục vụ cho việc phát lại, phân tích và machine learning đến AWS
   - Use case:
     - Smart home: Kết hợp với các thiết bị gia đình có trang bị camera như chuông, camera, webcam,...Sau đó có thế sử dụng dữ liệu để tạo ra các app thông minh gia đình (Ví dụ: Tương tác với chuông cửa hỗ trợ camera từ điện thoại di động của bạn)
     ![image](https://user-images.githubusercontent.com/57032236/180633334-2c375e6e-66db-442e-8317-ab877fada119.png)

     - Smart city: Kết hợp với camera tại đèn giao thông, bãi đỗ xe, trung tâm mua sắm,... để thu thập dữ liệu, có thể sử dụng để phân tích góp phần nào hạn chế tội phạm, vi phạm
     ![image](https://user-images.githubusercontent.com/57032236/180633309-39229ec8-47c9-4397-8db2-d8d0eacf9a14.png)
   
     - Tự động hóa công nghiệp: Có thể sử dụng để thu thập dữ liệu như tín hiệu RADAR, LIDAR, một vài dữ liệu ở thiết bị công nghiệp khác. Từ đó có tể phân tích dữ liệu bằng machine learning để sử dụng, ví dụ như dự báo tuổi thọ của van, đặt lịch thay thế linh kiện trước => giảm thiểu thời gian dừng hoạt động và lỗi dây chuyền sản xuất
     ![image](https://user-images.githubusercontent.com/57032236/180633355-05fc0169-ba43-452a-adce-ceaa63b83b8b.png)

 - #### Amazon Kinesis Data Streams
   - Thu thập log, dữ liệu cảm biến, dữ liệu nhấp chuột,...để phân tích, trả về output trong thời gian thực, ví dụ như bảng xếp hạng hoặc có thể chuyển data vào các hồ dữ liệu khác
   - Phân tích theo thời gian thực trong vài giây bằng lambda hoặc Amazon Kinesis Data Analystics
   ![image](https://user-images.githubusercontent.com/57032236/180633089-65f0f824-6fd7-4398-baae-5d3cba13b574.png)

 - #### Amazon Kinesis Data Firehose
   - Sử dụng để tải các stream data vào hồ dữ liệu, kho chứa hoặc dịch vụ phân tích khác theo thời gian thực (hiểu đơn giản là dịch vụ chuyển tiếp data cho kenesis)
   - Cho phép thu thập, chuyển đổi và tải data vào đích đến nào đó
   ![image](https://user-images.githubusercontent.com/57032236/180633192-cef46368-4269-4213-99ac-b05c814da870.png)
   - Usecase
     - Chuyển đổi, truyền data vào S3, hồ dữ liệu khác sang các định dạng khác mà không cần xây quy trình xử lý khác
     
 - #### Amazon Kinesis Data Analytics
   - Phân tích data cho dữ liệu phát trực tuyến trong thời gian thực bằng cách sử dụng Apache Flink
   ![image](https://user-images.githubusercontent.com/57032236/180633606-7d2ca76e-d305-48fd-8464-d1139db6e426.png)
<hr/>
 
### Elastic load balancer
 - Khi tạo load balancer => cần tạo target group => cần chỉ định target type => Khi tạo xong target group KHÔNG THỂ thay đổi target type
 - #### Target types
   - instance: chỉ định bởi các ID instance
   - ip: Chỉ định bởi các IP (Có thể đặt range dải mạng, etc: 10.0.0.0/8)
   - Lambda: Mục tiêu là 1 hàm lambda
 - Sử dụng header X-Forwarded-For để xem IP của client request nếu sử dụng load balancer
 - Error Code:
   - 503: Báo chưa có target (chưa đăng ký mục tiêu)
   - 504: Timeout
   - 403: bị chặn bởi AWS WAF
 - Khi ELB được tạo, nó sẽ nhận được tên DNS công khai, tên DNS này không thay đổi, nhưng public IP có thể thay đổi => nên sử dụng DNS thay vì public IP
<hr/>

### Auto Scaling
 - Health check + thay thế các unhealth instances
 - Không tự động attach volumn nếu ổ đĩa sắp hết dung lượng
 - Không tính phí
 - Khi sử dụng chung với ELB, cần đổi health check type từ EC2 -> ELB nếu không sẽ không tự replace unhealth instances
<hr/>
 
### S3:
 - Đối với event notification, nếu không bật chế độ version thì khi 2 request đều ghi vào 1 đối tượng => có thể sẽ chỉ có 1 event notification được trigger
 - Encryption
   - Client-side encryption: Object được mã hóa trước khi gửi lên S3
   - Server-side encryption: Việc mã hóa object được diễn ra ở phía server S3
     - SSE-S3: Mã hóa và khóa được quản lý bởi S3, mỗi object được mã hóa bằng 1 khóa duy nhất, sau đó mã hóa khóa đó bằng 1 khóa gốc khác, sử dụng tiêu chuẩn AES-256
     - SSE-KMS: Mã hóa ở phía S3, tuy nghiên sử dụng khóa KMS do chúng ta quản lý
     - SSE-C: Vẫn là mã hóa phía server tuy nhiên key là do client gửi lên (PHẢI SỬ DỤNG HTTPS)
<hr/>

### IAM
 - #### IAM access advisor
   - Sử dụng để scan / phân tích permission lần cuối được sử dụng bởi user / role là khi nào, từ đó có thể thấy được các quyền dư thừa => loại bỏ quyền dư thừa không sử dụng
 - #### IAM access analyzer
   - Sử dụng để phân tích các tài nguyên trong Organization hoặc trong tài khoản AWS để phát hiện các rủi ro bảo mật về chia sẻ tài nguyên ngoài ý muốn.
   - VD: 1 bucket S3 hoặc 1 role IAM chia sẻ với 1 thực thể khác bên ngoài => Thực thể đó có thể sử dụng để truy cập vào tài nguyên
 - Có thể sử dụng IAM để làm trình quẩn lý chứng chỉ (SSL/TLS) khi cần sử dụng HTTPS trong region không được ACM (AWS Certificate Manager) hỗ trợ
   - Chỉ sử dụng được đối với chứng chỉ SSL/TLS bên ngoài, không sử dụng được đối với chứng chỉ do ACM cung cấp
   - Không thể quản lý chứng chỉ ở bảng điều khiển
<hr/>

### DynamoDB
 - Sử dụng global table nếu có người dùng phân phối toàn cầu => giảm khoảng cách vật lý giữa client và DynamoDB endpoint => Giảm độ trễ
 - #### Eventually consistent
   - Response có thể sẽ không phải data mới nhất, có thể hiểu rằng response sẽ là data tại lúc call, trong quá trình call, data có được thay đổi cũng sẽ không được trả về data mới nhất => nhanh hơn so với strongly consistent
 - #### Strongly consistent
   - Trả về dữ liệu được cập nhật mới nhất, tuy nhiên sẽ đi kèm với 1 số nhược điểm:
     - Có độ trễ cao hơn so với eventually consistent
     - Không hỗ trợ đối với global secondary indexes (chỉ mục thứ cấp toàn cầu)
     - Sử dụng nhiều read capacity hơn so với eventually consistent
 - DynamoDB mặc định sử dụng Eventually consistent, trừ khi setting khác
 - Các câu lệnh GetItem, Query, Scan cho phép thêm option ConsistentRead = true để sử dụng Strongly Consistent trong quá trình thao tác.
 - #### Amazon DynamoDB Accelerator (DAX)
   - Là bộ nhớ đệm, có khả năng sử dụng cao, được thiết kế riêng cho DynamoDB
   - Cải thiện performance lên đến 10 lần - từ mili second -> micro second ngay cả khi có hàng triệu request mỗi giây
   - Tính phí theo giờ và các instance DAX không cần cam kết dài hạn
 - #### request options:
   - ProjectionExpression: Sử dụng để filter attributes thay vì get all attributes
   - FilterExpression: Thêm điều kiện lọc records
<hr/>

### RDS (Relationship database service)
 - là một tập hợp các dịch vụ database
   - Aurora
     - MySQL
     - PostgreSQL
   - MySQL
   - MariaDB
   - PostgreSQL
   - Oracle
   - SQL Server
   - Outposts (Cho phép toàn quyền quản lý database RDS ở môi trường On-Premises)
 - RDS Multi-AZ
   - áp dụng các bản patch cập nhật OS bằng cách upgrade ở standby -> chuyển standby thành primary -> upgrade old primary -> chuyển old primary thành standby
   - Ưu điểm
     - Tăng độ sẵn sàng (các AZ độc lập về mặt vật lý -> nếu chẳng may có AZ lỗi thì vẫn có bản dự phòng)
     - Nâng cao độ bền (Sao chép đồng bộ giữa nhiều AZ -> luôn được cập nhật theo phiên bản chính)
     - Độ sẵn sàng cao (Chuyển đổi CSDL dự phòng nếu có vấn đề xảy ra trong vòng 60s mà không cần can thiệp thủ công)
<hr/>

### Elastic Beanstalk
 - Sử dụng để triển khai, mở rộng các web application được viết bằng Java, .NET, PHP, Node.js, Ruby, Python, Go và Docker trên những máy chủ: Apache, Nginx, Passenger và IIS
 - Chỉ cần upload code lên mà không cần lo về vấn đề deployment, performance về mặt infra, monitoring,...
 - Vẫn có quyền access vào các resources
 - Không tính thêm phí, chỉ tính phí các tài nguyên sử dụng như: EC2, ECS,...
 - #### Deployment stragies
   - All-at-Once: Tất cả instances trong 1 lần
     ![image](https://user-images.githubusercontent.com/57032236/180630279-cb57e0fb-59c5-48c7-add6-2fb153e2117c.png)
     
   - Rolling: Deploy thành nhiều đợt cuốn chiếu
     ![image](https://user-images.githubusercontent.com/57032236/180630281-b6908b64-4d07-49dd-bd16-f2ca3db200e3.png)
     
   - Rolling with Additional Batch: Tách ra thành nhiều lô, lô đầu tiên sẽ tạo ra số instance EC2 mới thay vì các instances EC2 đã có (1 lô có thể chỉ định số lượng instances).
     ![image](https://user-images.githubusercontent.com/57032236/180630288-3cb97a08-0658-4619-849a-97208d6994f1.png)

   - Immutable: Triển khai toàn bộ instance mới, sau đó sẽ swap instances mới với instance cũ
     ![image](https://user-images.githubusercontent.com/57032236/180630302-81cb5f2e-a36f-4b76-a8a9-20b80820e4e0.png)

   - Traffic Splitting: Deploy theo chiến lược Immutable tuy nhiên sẽ cắt số traffic nhất định đến instances mới để thử nghiệm, nếu hoạt động tốt sẽ chuyển 100% lưu lượng sang instances mới
   - Blue/Green (đọc bên dưới trong AWS CodeDeploy)
<hr/>

### CloudTrail
 - Theo dõi và ghi lại hoạt động của ACCOUNT đối với các resource AWS => khi cần check lại thay đổi resource theo scope USER NÀO thay đổi => nghĩ đến cloudtrail
 - NOTE
   - cloudtrail sẽ không ghi lại sự kiện 'CreateVolume' khi EBS được tạo trong quá trình launch EC2
<hr/>

### AWS CodeDeploy
 - Hỗ trợ auto deploy
 - Sử dụng file appspec.yml/appspec.yaml trong root-directory
 - Các kiểu deploy
   - In-place deployment
     - Application trong nhóm target deploy sẽ bị dừng và deploy code, settings,... mới nhất rồi sau đó khởi động lại
     - Chỉ hoạt động đới với EC2 / On-premises
   - Blue-Green deployment: Giải quyết vấn đề downtime, mục đích và ý tưởng là sử dụng 2 tài nguyên phần cứng song song giống hệt nhau để tạo ra 2 môi trường production (blue và green).
     - VD
       - User đang sử dụng version code 1 (Blue)
       - Version mới được deploy lên môi trường giống hệt blue => môi trường green
       - Sau khi deploy xong nếu check OK => route traffic qua môi trường mới (green)
       ![image](https://user-images.githubusercontent.com/57032236/180630810-05112a08-df77-4f7b-88c4-c0237e582d35.png)

     - Ưu điểm:
       - Zero downtime
       - Dễ dàng rollback vì đã có sẵn môi trường blue
<hr/>

### AWS CodeBuild
- Hỗ trợ define các step buildcode
- Sử dụng file buildspec.yml trong root-directory
- Sử dụng local build bằng CodeBuild Agent để: test, debug
<hr/>

### CloudFormation
 - Viết file quản lý cơ sở hạ tầng, cung cấp và mô tả các tài nguyên AWS cũng như bên thứ 3. VD: Cung cấp VPC, subnet, EC2, ECS, CI/CD,...
   ```
   {
     "AWSTemplateFormatVersion" : "2010-09-09",
     "Description" : "A sample template",
     "Resources" : {
       "MyEC2Instance" : {
         "Type" : "AWS::EC2::Instance",
         "Properties" : {
           "ImageId" : "ami-2f726546",
           "InstanceType" : "t1.micro",
           "KeyName" : "testkey",
           "BlockDeviceMappings" : [
             {
               "DeviceName" : "/dev/sdm",
               "Ebs" : {
                 "VolumeType" : "io1",
                 "Iops" : "200",
                 "DeleteOnTermination" : "false",
                 "VolumeSize" : "20"
               }
             }
           ]
         }
       }
     }
   }

   ```
   Nhìn vào template trên sẽ hiểu được chúng ta định nghĩa ra 1 instance EC2 với AMI id là "ami-2f726546", instance type là "t1.micro", key pair là "testkey" và một EBS có type là "iop1",...
   ![image](https://user-images.githubusercontent.com/57032236/180631172-4de1791b-2f3b-4165-af90-8831378b90da.png)
   
 - Stack: Đơn vị dùng để gọi 1 nhóm các tài nguyên được định nghĩa bởi 1 template, chẳng hạn như khi có 1 file cloudFormation bao gồm các resources: ASG, ELB, RDS,...tạo các resources trên bằng AWS cloudFormation sẽ tương đương với 1 stack
 - Sử dụng output Export xuất giá trị đầu ra để có thể tái sử dụng ở 1 stack khác
 - Function:
   - !GetAtt/Fn::GetAtt => Trả về giá trị của 1 attribute từ một tài nguyên trong template
   - !Sub/Fn::Sub => Thay thế các biến trong chuỗi đầu vào bằng các giá trị chỉ định
   - !Ref => Trả về giá trị của tham số hoặc tài nguyên được chỉ định
   - !FindInMap/Fn::FindInMap trả về giá trị tương ứng với các key
<hr/>

### CloudFront
 - CloundFront route tất cả request đến primary origin, chỉ gửi request đến origin phụ khi một request đến origin chính không thành công
<hr/>

### ECS
 - Để config ECS, viết config trong file /etc/ecs/ecs.config
   - ECS_ENABLE_TASK_IAM_ROLE: Sử dụng để kích hoạt IAM role cho container
<hr/>   

### EBS
 - Nếu bật chế độ Encryption by default, từ sau đó trở đi các EBS mới được tạo ra sẽ được mã hóa theo mặc định
   - Mã hóa theo mặc định là theo region, không thể chỉ định đặc biệt EBS nào được mã hóa EBS nào không trong cùng 1 region
<hr/>

### AWS Secret Manager
 - Sử dụng để quản lý các bí mật cần thiết để truy cập các ứng dụng, dịch vụ và tài nguyên AWS, ví dụ như mật khẩu database, API key,...
 - Cho phép xoay vòng
 - Truy xuất dữ liệu trong AWS Secret Manager bằng cách call API
<hr/>

### AWS Trusted advisor
 - Là một công cụ trực tuyến cung cấp phương pháp cải thiện in real-time theo best practice của AWS về:
   - cost optimization (tối ưu hóa chi phí)
   - security (bảo mật)
   - fault tolerance (khả năng chịu lỗi)
   - service limits (giới hạn dịch vụ một cách đơn giản nhất)
   - and performance improvement (cải thiện hiệu suất).
<hr/>

### AWS Inspector
 - Là một dịch vụ đánh giá bảo mật tự động giúp cải thiện tính bảo mật và tuân thủ của các application được deploy trên AWS
 - Đánh giá về:
   - exposure (mức độ phơi nhiễm)
   - vulnerabilities (lỗ hổng bảo mật)
   - deviations (độ sai lệch so với best practice)
<hr/>

### AWS CDK
 - Sử dụng để định nghĩa các resources bằng các ngôn ngữ lập trình quen thuộc như: python, java, js,...
<hr/>

### AWS Budget
 - Cần dữ liệu của 5 tuần để có thể dự báo ngân sách
<hr/>
