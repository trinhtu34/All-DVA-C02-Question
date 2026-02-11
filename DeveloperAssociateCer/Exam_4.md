## Câu 1: Ứng dụng mobile của bạn cần thực hiện API call tới DynamoDB. Bạn không muốn lưu AWS Secret và access keys vào thiết bị mobile và cần tất cả call tới DynamoDB làm cùng với các identity khác nhau trên các thiết bị khác nhau 

Service nào sau đây cho phép bạn đạt được điều này ? 

**Đúng**
- Cognito Identity Pools

**Sai**
- Cognito User Pools
- Cognito Sync
- IAM

**Giải thích**



## Câu 2: 1 nền tảng giao tiếp phục vụ hàng triệu khách hàng và triển khai tính năng lên 1 prod env trên AWS thông qua CodeDeploy. Bạn đang review các script để đặt việc deploy vào AppSpec file.

Lựa chọn danh sách nào sau đây là thứ tự hợp lệ cho vòng đời của event ? 

**Đúng**
- DownloadBundle => BeforeInstall => ApplicationStart => ValidateService

**Sai**
- ValidateService => BeforeInstall =>DownloadBundle => ApplicationStart
- BeforeInstall => ApplicationStart => DownloadBundle => ValidateService
- BeforeInstall => ValidateService =>DownloadBundle => ApplicationStart

**Giải thích**

Lifecycle Event Hook Availability

| Lifecycle event name | In-place deployment | Blue/Green Deployment: Original instances | Blue/Green deployment: replacement instances | Blue/Green deployment rollback: Original instance| Blue/Green deployment rollback: Replacement instance|
|---|---|---|----|---|---|
|ApplicationStop|x||x|||
|DownloadBundle|x||x|||
|BeforeInstall|x||x|||
|Install|x||x|||
|AfterInstall|x||x|||
|ApplicationStart|x||x|||
|ValidateService|x||x|||
|BeforeBlockTraffic|x|x|||x|
|BlockTraffic|x|x|||x|
|AfterBlockTraffic|x|x|||x|
|BeforeAllowTraffic|x||x|x||
|AllowTraffic|x||x|x||

## Câu 3: 1 tổ chức gần đây đã bắt đầu sử dụng CodeCommit cho source control service. 1 team tuân thủ bảo mật đã đến kiểm tra quy trình phát triển phần mềm và nhận thấy các dev làm nhiều lệnh git push trên máy dev. Team tuân thủ yêu cầu mã hóa được dùng cho hành động này  

Tổ chức làm thế nào để đảm bảo source code được mã hóa `in transit` và `rest` không ?

**Đúng**
- Repositories are automatically encrypted at rest

**Sai**
- Use AWS Lambda as a hook to encrypt the pushed code
- Enable KMS encryption
- Use a git command line hook to encrypt the code client side

**Giải thích**



## Câu 4: Bạn đã di chuyển infra ở on-premises của bạn lên AWS và trong quá trình cấu hình 1 môi trường dev của AWS Elastic Beanstalk cho Production, development, testing. Bạn đã cấu hình môi trường production của bạn để dùng 1 rolling deployment để hạn chế ứng dụng của bạn trở nên unavailable với users. Đối vối development và testing env, bạn muốn triển khai nhanh chóng và không lo ngại về downtime.

Deployment policies nào sau đây đáp ứng điều bạn cần ? 

**Đúng**
- All at once

**Sai**
- Immutable
- Rolling with additional batches
- Rolling

**Giải thích**



## Câu 5: Bạn có 1 website phổ biến, nó truy cập dữ liệu lưu trong S3 bucket. Các dev sử dụng SDK để duy trì ứng dụng và thêm tính năng mới. Yêu cầu tuân thủ bảo mật ở tất cả object được up lên S3 phải được mã hóa dùng SSE-S3 tại thời điểm upload. Headers nào dưới đây phải được dev thêm vào request ? 

**Đúng**
- 'x-amz-server-side-encryption': 'AES256'

**Sai**
- 'x-amz-server-side-encryption': 'SSE-KMS'
- 'x-amz-server-side-encryption': 'aws:kms'
- 'x-amz-server-side-encryption': 'SSE-S3'

**Giải thích**



## Câu 6: Thứ tự của Hooks cho in-place deployments sử dụng CodeDeploy là gì ? 

**Đúng**
- Application Stop -> Before Install -> Application Start -> ValidateService

**Sai**
- Before Install -> Application Stop -> ValidateService -> Application Start
- Application Stop -> Before Install -> ValidateService -> Application Start
- Before Install -> Application Stop -> Application Start -> ValidateService

**Giải thích**



## Câu 7: 1 công ty thương mại điện tử đã triển khai CodeDeploy là 1 phần của chiến lược AWS Cloud CI/CD. Công ty đã cấu hình tự động rollback trong khi deploy 1 phiến bản mới của ứng dụng cao cấp của họ trên EC2.

Chuyện gì xảy ra nếu deploy phiên bản mới bị fail ?

**Đúng**
- A new deployment of the last known working version of the application is deployed with a new deployment ID

**Sai**
- AWS CodePipeline promotes the most recent working deployment with a SUCCEEDED status to production
- The last known working deployment is automatically restored using the snapshot stored in Amazon S3
- CodeDeploy switches the Route 53 alias records back to the known good green deployment and terminates the failed blue deployment

**Giải thích**



## Câu 8: 1 công ty IT có 1 web app đang chạy trên EC2 instance cần read-only access tới 1 DynamoDB table. Là 1 DVA, giải pháp nào là best practice bạn sẽ khuyên để đáp ứng công việc này ?

**Đúng**
- Create an IAM role with an AmazonDynamoDBReadOnlyAccess policy and apply it to the EC2 instance profile

**Sai**
- Create a new IAM user with access keys. Attach an inline policy to the IAM user with read-only access to DynamoDB. Place the keys in the code. For security, redeploy the code whenever the keys rotate
- Run application code with AWS account root user credentials to ensure full access to all AWS services
- Create an IAM user with Administrator access and configure AWS credentials for this user on the given EC2 instance

**Giải thích**



## Câu 9: bạn là 1 System Admin, công ty gần đây đã di chuyển production application lên AWS và đã migrate dữ liệu từ MySQL lên DynamoDB. Bạn đã thêm 1 tables mới ở DynamoDB và cần cho phép ứng dụng của bạn truy vấn dữ liệu bằng primary key và 1 alternate key. Lựa chọn này phải được thêm khi tạo Table lần đầu, nếu không sẽ không thể thực hiện sau đó. 

Hành động nào sau đây bạn nên làm ? 

**Đúng**
- Create a LSI

**Sai**
- Migrate away from DynamoDB
- Create a GSI
- Call Scan

**Giải thích**

`LSI` là tiêu chuẩn cho Local Secondary Index. Một số ứng dụng chỉ cần query data sử dung primary key cơ bản của table; tuy nhiên có thể có trường hợp mà alternate sort key có thể có ích hơn. Để cho ứng dụng của bạn chọn sort keys, bạn có thể tạo 1 hoặc nhiều Local Secondary Index trên 1 table và phát hành Query hoặc Scan request đối với các indexes này. 

## Câu 10: 1 hệ thống đánh giá được host ở on-premises gần đây đã migrate lên AWS để giảm chi phí, tăng scalability, và phục vụ hàng ngàn người dùng lúc. Khi 1 trong các trạng thái của AWS Resource thay đổi, nó tạo ra 1 event và sẽ cần trigger AWS Lambda. AWS Resource thay đổi trạng thái và Lambda không có tích hợp trực tiếp gì với nhau. 

Methods nào sau đây có thể được dùng để trigger AWS Lambda ? 

**Đúng**
- Amazon EventBridge rules with AWS Lambda

**Sai**
- AWS Lambda Custom Sources
- Cron jobs to trigger AWS Lambda to check the state of your service
- Open a support ticket with AWS

**Giải thích**



## Câu 11: Công ty của bạn được thuê để xây dựng 1 ứng dụng mobile đánh giá có khả năng phục hồi cho 1 lễ trao giải âm nhạc sắp tới, nó ghi nhận có 5 tới 20 triệu lượt xem. Ứng dụng đánh giá trên điện thoại sẽ được quảng bá rầm rộ nhiều tráng trước đó vì vậy bạn kỳ vọng sẽ xử lý hàng triệu tin nhắn trong hệ thống. Bạn đang cấu hình SQS queues cho kiến trúc của bạn nên nhận messages từ 20Kib tới 200KiB. Có thể gửi tin nhắn này tới SQS không ? 

**Đúng**
- Yes, the max message size is 1024 KiB

**Sai**
- No, the max message size is 64 KiB
- No, the max message size is 128 KiB
- Yes, the max message size is 512 KiB

**Giải thích**

Kích thước tối thiểu là 1 byte (1 ký tự). Tối đa là 1.048.576 bytes (1024KiB)

## Câu 12: 1 nhà cung cấp giáo dục toàn cầu vận hành ứng dụng Hệ thống quản lý học tập (LMS) trên EC2 instances sau 1 ALB, cùng với domain name được quản lý trong Route 53. LMS phụ thuộc lớn vào các static assets như images, style sheets, và JS files, và ứng dụng hiện tại được deploy trên 1 single AWS Region. Nhà cung cấp muốn Performance của deliver nhanh hơn tới học sinh trên toàn thế giới trong khi tối thiểu chi phí hoạt động thường xuyên .

Giải pháp nào sau đây sẽ cải thiện global performance cùng với nỗ lực hoạt động ít nhất 

**Đúng**
- Create an Amazon CloudFront distribution with the ALB as the origin, and point the application’s Route 53 alias to the CloudFront domain

**Sai**
- Enable Amazon S3 Transfer Acceleration and move static assets to S3; update the application to fetch assets via the accelerated S3 endpoint
- Deploy the application in multiple Regions and use Route 53 latency-based routing to direct users to the nearest ALB
- Put AWS Global Accelerator in front of the ALB and update Route 53 to alias the app’s domain to the accelerator

**Giải thích**

Vì CloudFront có Edge locations toàn cầu, Cache tốt, Fully Managed, có thể dùng ALB làm Origin, không cần deploy multi-region, không cần thay đổi application nhiều. 

## Câu 13: Team dev tại 1 công ty muốn thêm bản ghi cho Vendor vào 1 DynamoDB table càng sớm càng tốt khi Vendor upload 1 file mới lên S3 bucket. Là 1 DVA, Nhóm các bước nào sau đây mà bạn sẽ khuyên để đạt được điều này ?  

**Đúng**
- Create an S3 event to invoke a Lambda function that inserts records into DynamoDB

**Sai**
- Write a cron job that will execute a Lambda function at a scheduled time and insert the records into DynamoDB
- Develop a Lambda function that will poll the S3 bucket and then insert the records into DynamoDB
- Set up an event with Amazon EventBridge that will monitor the S3 bucket and then insert the records into DynamoDB

**Giải thích**

Tạo 1 S3 event để invoke 1 Lambda function để thêm records vào DynamoDB

## Câu 14: 1 công ty IT sử dụng Blue/Green Deployment policy để cấp phát EC2 instances mới trong 1 ASG sau 1 ALB mới cho mỗi phiên bản mới của ứng dụng. Hiện tại setup yêu cầu người dùng đăng nhập mỗi lần deploy mới.

Là DVA, bạn sẽ khuyên công ty gì để giải quyết vấn đề ? 

**Đúng**
- Use ElastiCache to maintain user sessions

**Sai**
- Use multicast to replicate session information
- Use rolling updates instead of a blue/green deployment
- Enable sticky sessions in the Application Load Balancer

**Giải thích**



## Câu 15: Bạn có 1 ứng dụng được xây dựng dựa trên Java vận hành trên EC2 instances được load cùng với CodeDeploy agents. Bạn đang cân nhắc các lựa chọn deployment khác, 1 trong các ưu điểm là tính linh hoạt cho phép tăng dầncác phiên bản của ứng dụng mới và thay thế các phiên bản đang có trong EC2. Lựa chọn khác là 1 chiến lược trong ASG được dùng để thực hiện deployment.

Lựa chọn nào sau đây sẽ cho phép bạn deploy cách này ? 

**Đúng**
- In-place Deployment
- Blue/green Deployment

**Sai**
- Cattle Deployment
- Warm Standby Deployment
- Pilot Light Deployment

**Giải thích**

In-place deployment Là ứng dụng trên mỗi instance trong deployment group bị stop, phiên bản gần nhất của ứng dụng được tải xuống, và mỗi phiên bản của ứng dụng được start và validate. Bạn có thể sử dụng 1 LB vì vậy với mỗi instance được deregister trong thời gian deployment và sau đó được restored service sau khi deployment hoàn tất. 

## Câu 16: 1 công ty muốn thêm khả năng không gian địa lý vào cache layer, cùng với khả năng truy vấn và khả năng mở rộng theo chiều ngang. Công ty sử dụng RDS là database tier.

Giải pháp nào là tối ưu cho trường hợp này ? 

**Đúng**
- Leverage the capabilities offered by ElastiCache for Redis with cluster mode enabled

**Sai**
- Migrate to Amazon DynamoDB to utilize the automatically integrated DynamoDB Accelerator (DAX) along with query capability features
- Use CloudFront caching to cater to demands of increasing workloads
- Leverage the capabilities offered by ElastiCache for Redis with cluster mode disabled

**Giải thích**



## Câu 17: Công ty của bạn quản lý MySQL database trên EC2 instance để có full control. Ứng dụng trên EC2 instances được quản lý bởi 1 ASG cho các request tới database lấy thông tin để hiển thị dữ liệu lên dashboards được xem bởi ứng dụng điện thoại, table, và web browsers.

Quản lý của bạn muốn cale ASG của bạn dựa vào số request/m. Bạn làm thế nào để đạt điều này ? 

**Đúng**
- You create a CloudWatch custom metric and build an alarm to scale your ASG

**Sai**
- Attach additional Elastic File Storage
- Attach an Elastic Load Balancer
- You enable detailed monitoring and use that to scale your ASG

**Giải thích**

Ở đây cần metric `number of requests per minute`, cái mà là custom metric chúng ta cần tạo, vì metrics này không có sẵn trong CloudWatch

## Câu 18: Bạn có 1 KDS với 10 shards, và từ metrics, bạn thấy mức sử dụng của bạn thấp hơn nhiều so với 10MB/s để gửi dữ liệu. Bạn gửi 3MB/s dữ liệu và bạn nhận errors `ProvisionedThroughputExceededException` thường xuyên. Lý do của vấn đề này là gì ? 

**Đúng**
- The partition key that you have selected isn't distributed enough

**Sai**
- Metrics are slow to update
- You have too many shards
- The data retention period is too long

**Giải thích**

KDS có 10 shards, tổng là 10MB/s, thực tế thì sử dụng chỉ có 3MB/s, nhưng vẫn báo lỗi `ProvisionedThroughputExceededException` chắc chắn là do phân phối dữ liệu không đồng đều giữa các shards

Lý do dẫn tới hiện tượng này là: chọn partition kém, Kinesis không round-robin giữa các shard. Nếu partition key: ít giá trị, lặp lại, có tính tuần tự, có tính gom nhóm thì dữ liệu có thể bị dồn vào 1 hoặc 1 vài shards

Để giải quyết vấn đề này thì phải có nhiều partition key, hoặc partition key có độ phân bố cao

## Câu 19: Là 1 phần của quy định nội bộ, bạn phải đảm bảo tất cả các communication tới S3 được mã hóa. Cơ chế mã hóa nào sau đây sẽ từ chối 1 request nếu kết nối đó không dùng HTTPS

**Đúng**
- SSE-C

**Sai**
- SSE-KMS
- Client Side Encryption
- SSE-S3

**Giải thích**

S3 sẽ từ chối bất kỳ request nào thông qua HTTP khi sử dụng SSE-C. 

## Câu 20: 1 Dev đang cấu hình 1 ALB để traffic đi trực tiếp từ EC2 instance của ứng dụng và lambda function.

Đặc trưng nào sau đây của ALB có thể được xác định là hợp lệ 

**Đúng**
- You can not specify publicly routable IP addresses to an ALB
- An ALB has three possible target types: Instance, IP and Lambda

**Sai**
- If you specify targets using an instance ID, traffic is routed to instances using any private IP address from one or more network interfaces
- If you specify targets using IP addresses, traffic is routed to instances using the primary private IP address
- An ALB has three possible target types: Hostname, IP and Lambda

**Giải thích**

 Khi target type là IP, bạn chỉ có thể chỉ định địa chỉ IP từ CIDB block. Bạn không thể chỉ định địa chỉ IP cụ thể. 

## Câu 21: Bạn đang lên kế hoạch xây dựng 1 nhóm EC2 instance có EBS được optimized để xử lý load của ứng dụng mới. Do vấn đề liên quan tới tuân thủ bảo mật, tổ chức của bạn muốn bất kỳ secret strings nào trong ứng dụng phải được mã hóa để hạn chế expose giá trị đó ra ngoài dưới dạng text.

Giải pháp yêu cầu mã hóa event được audit và API call đơn giản. Làm thế nào để đạt được điều này ? 

**Đúng**
- Audit using CloudTrail
- Store the secret as SecureString in SSM Parameter Store

**Sai**
- Store the secret as PlainText in SSM Parameter Store
- Audit using SSM Audit Trail
- Encrypt first with KMS then store in SSM Parameter Store

**Giải thích**

CloudTrail sẽ ghi lại `ssm:GetParameter`, `kms:Decrypt` nó sẽ đáp ứng yêu cầu Descryption events be audited

SecureString sẽ tự động mã hóa bằng AWS KMS, khi gọi API: `aws ssm get-parameter --with-descryption` API đơn giản, không cần tự encrypt/descrypt

## Câu 22: 1 team dev đang lưu trữ dữ liệu nhạy cảm của khách hàng trong S3 sẽ yêu cầu mã hóa ở nơi lưu trữ ( encyption at rest ). Encryption keys phải được xoay hàng năm là ít nhất.

Cách gì là dễ nhất để triển khai 1 giải pháp cho yêu cầu này ? 

**Đúng**
- Use AWS KMS with automatic key rotation

**Sai**
- Encrypt the data before sending it to Amazon S3
- Import a custom key into AWS KMS and automate the key rotation on an annual basis by using a Lambda function
- Use SSE-C with automatic key rotation on an annual basis

**Giải thích**



## Câu 23: 1 tổ chức sử dụng Alexa làm trợ lý thông minh để cải thiện hiệu suất làm việc trong toàn bộ tổ chức. 1 nhóm các dev quản lý custom Alexa Skills được viết bằng Node.Js để control các thiết bị phòng họp và bắt đầu các cuộc họp bằng giọng nói. Người quản lý đã yêu cầu các dev code các tính năng nên được theo dõi tỉ lệ lỗi cùng với khả năng tạo ra các cảnh báo trên đó 

Lựa chọn nào nên được chọn ? 

**Đúng**
- CloudWatch Metrics
- CloudWatch Alarms

**Sai**
- CloudTrail
- X-Ray
- SSM

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 24: 1 công ty phân tích dữ liệu cùng với IT infra của họ trên AWS muốn xây dựng và deploy ứng dụng cao cấp của họ càng sớm càng tốt khi có bất kỳ thay đổi về code nào ?

Là 1 DVA, lựa chọn nào bạn sẽ đề xuất để trigger deployment ? 

**Đúng**
- Keep the source code in an Amazon S3 bucket and start AWS CodePipeline whenever a file in the S3 bucket is updated
- Keep the source code in an AWS CodeCommit repository and start AWS CodePipeline whenever a change is pushed to the CodeCommit repository

**Sai**
- Keep the source code in an Amazon S3 bucket and set up AWS CodePipeline to recur at an interval of every 15 minutes
- Keep the source code in an Amazon EBS volume and start AWS CodePipeline whenever there are updates to the source code
- Keep the source code in Amazon EFS and start AWS CodePipeline whenever a file is updated

**Giải thích**



## Câu 25: 1 dev đang migrate 1 ứng dụng ở on-premises lên AWS. Ứng dụng hiện tại xử lý upload và upload chúng tới 1 thư mục local ở server. Tất cả file upload phải được lưu vào sau đó khả dụng cho tất cả instances trong 1 ASG.

Là 1 DVA, lựa chọn nào bạn sẽ đề xuất cho trường hợp này ? 

**Đúng**
- Use Amazon S3 and make code changes in the application so all uploads are put on S3

**Sai**
- Use Amazon EBS as the storage volume and share the files via file synchronization software
- Use Amazon EBS and configure the application AMI to use a snapshot of the same EBS instance while launching new instances
- Use Instance Store type of EC2 instances and share the files via file synchronization software

**Giải thích**

Lựa chọn EBS multi-attach thì dùng được nhưng rất hạn chế. Vì vậy để tốt nhất cho trường hợp này thì dùng S3 để lưu file.

## Câu 26: team dev tại 1 công ty đang tìm cách xây dựng 1 CloudFormation template tự động điền thông tin AWS Region variable trong khi triển khai CloudFormation template.

Cách nào là hiệu quả nhất về mặt vận hành để quyết định Region mà template đang được triển khai

**Đúng**
- Use the AWS::Region pseudo parameter

**Sai**
- Create a CloudFormation parameter for Region and let the desired value be populated at the time of deployment
- Create an AWS Lambda-backed custom resource for Region and let the desired value be populated at the time of deployment by the Lambda
- Set up a mapping containing the key and the named values for all AWS Regions and then have the CloudFormation template auto-select the desired value

**Giải thích**

User không cần nhập, không cần code thêm, không cần maintain => chọn pseudo parameter. Vì AWS::Region là Built-in pseudo parameter, tự động có sẵn khi CloudFormation chạy, luôn đúng 
region đang deploy. Example:

```yaml
Resources:
  MyBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub my-bucket-${AWS::Region}

```

## Câu 27: 1 dev team sử dụng AWS SDK for Java để bảo trì 1 ứng dụng lưu dữ liệu trong AWS DynamoDB. Ứng dụng sử dụng `Scan` operations để trả về nhiều items từ 1 table 25GiB. Không thể tạo Indexex để truy xuất các items có thể dự đoán được. Các dev đang cố gắng thử lấy dữ lileuej từ các dòng được chỉ định từ DynamoDB càng nhanh càng tốt. 

Lựa chọn nào sau đây có thể được sử dụng để cải thiện performance của Scan operations ? 

**Đúng**
- Use parallel scans

**Sai**
- Use a FilterExpression
- Use a ProjectionExpression
- Use a Query

**Giải thích**

Yêu cầu: Phải dùng Scan, table 25GB, không thể tạo indexes, nhanh nhất có thể => dùng Parallel để giảm total scan time vì Scan song song. 

## Câu 28: 1 SQS đã được cấu hình giữa 2 AWS Accounts để truy cập các shared trong queue. AWS Account A có SQS queue trong account của nó và AWS Account B có truy cập vào queue trong Account A. Lựa chọn nào sau đây cần thiết để kết hợp để cho phép truy cập trên nhiều account ? 

**Đúng**
- The account A administrator attaches a trust policy to the role that identifies account B as the principal who can assume the role
- The account B administrator delegates the permission to assume the role to any users in account B
- The account A administrator creates an IAM role and attaches a permissions policy

**Sai**
- The account A administrator delegates the permission to assume the role to any users in account A
- The account B administrator creates an IAM role and attaches a trust policy to the role with account B as the principal
- The account A administrator attaches a trust policy to the role that identifies account B as the AWS service principal who can assume the role

**Giải thích**
```
User (Account B)
   ↓ assume role
IAM Role (Account A)
   ↓ permissions
SQS Queue (Account A)
```

## Câu 29: Bạn là 1 quản lý cho 1 công ty công nghệ vừa thuê 1 team Devs làm việc trên AWS infra của công ty. Tất cả các devs báo về bạn khi dùng CLI thực thi lệnh bị fail cùng với exception như sau: `You are not authorized to perform this operation`. Encoded authorization failure message: `6h34GtpmGjJJUm946eDVBfzWQJk6z5GePbbGDs9Z2T8xZj9EZtEduSnTbmrR7pMqpJrVYJCew2m8YBZQf4HRWEtrpncANrZMsnzk`.

Hành động nào sau đây sẽ giúp các dev decode message ? 

**Đúng**
- AWS STS decode-authorization-message

**Sai**
- AWS IAM decode-authorization-message
- Use KMS decode-authorization-message
- AWS Cognito Decoder

**Giải thích**

Sử dụng decode-authorization-message để decode thông tin thêm về trạng thái xác thực của 1 request từ 1 encoded message được trả về trong response tới 1 AWS request. Nếu 1 user không được xác thực để thực hiện 1 hành động được yêu cầu, request sẽ trả về 1 lỗi 403 - Client.UnauthorizedOperation. Message đucợ mã hóa bởi vì chi tiết của trạng thái xác thực có thể cấu thành từ thông tin đặc quyền của người dùng, người mà yêu cầu thực hiện hành động không được phép. Để giải mã 1 message trạng thái xác thực, 1 người dùng phải được gán quyền thông qua IAM policy để reqeust sts:DecodeAuthorizationMessage action

## Câu 30: Bạn được thuê vào 1 công ty cần 1 dev có kinh nhiệm để giúp làm CI/CD workflow trên AWS. Bạn cấu hình workflow của công ty để chạy 1 AWS CodePipline pipline bất kể khi nào source code của ứng dụng trên repository được host ở AWS CodeCommit và complies source code với AWS CodeBuild. Bạn đang cấu hình ProjectArtifacts trong build stage của bạn.

Bạn nên chọn lựa chọn nào sau đây ? 

**Đúng**
- Give AWS CodeBuild permissions to upload the build output to your Amazon S3 bucket

**Sai**
- Give AWS CodeCommit permissions to upload the build output to your Amazon S3 bucket
- Configure AWS CodeBuild to store output artifacts on EC2 servers
- Contact AWS Support to allow AWS CodePipeline to manage build outputs

**Giải thích**



## Câu 31: Bạn đang làm việc trên 1 dự án có hơn 100 dependencies. Mỗi làn CodeBuild của bạn chạy 1 build step để resolve Java dependencies từ các repoitories Ivy bên ngoài, điều này mất rất nhiều thời gian. Quản lý của bạn muốn tăng tốc xử lý trong AWS CodeBuild.

Lựa chọn nào sau đây sẽ giúp bạn làm điều này cùng với nỗ lực thấp nhất ? 

**Đúng**
- Cache dependencies on S3

**Sai**
- Ship all the dependencies as part of the source code
- Reduce the number of dependencies
- Use Instance Store type of EC2 instances to facilitate internal dependency cache

**Giải thích**

Download dependencies là 1 phase quan trọng trong quá trình Build. Các file dependencies này có khoảng size từ 1 vài KBs tới nhiều MBs. Bởi vì hầu hết dependencies không thay đổi thường xuyên giữa các lần Build, bạn có thể giảm thời gian build của bạn bằng cách caching dependencies trong S3. 

## Câu 32: 1 Công ty sử dụng AWS DynamoDB để lưu trư thông tin về các đội sports yêu thích của mọi người và cho phép thông tin này có thể tìm kiếm từ home page của họ. Có 1 yêu cầu hàng ngày khoảng 10 triệu bản ghi trong table nên được deleted sau đó reload vào lúc 2:00 AM mỗi đêm.

Lựa chọn nào là cách hiệu quả để xóa cùng với chi phí thấp nhất ? 

**Đúng**
- Delete then re-create the table

**Sai**
- Scan and call DeleteItem
- Call PurgeTable
- Scan and call BatchDeleteItem

**Giải thích**

Hành động DeleteTable xóa 1 table và tất cả items của nó. Sau khi 1 request `DeleteTable` được thực hiện, table được chỉ định sẽ vào trạng thái `DELETING` cho tới khi DynamoDB hoàn tất quá trình delete. 

## Câu 33: 1 team dev đang cân nhắc dùng ElastiCache for Redis là in-memory caching solution cho relational database của nó.

Lựa chọn nào sau đây là hợp lệ trong khi đang cấu hình ElatiCache ? 

**Đúng**
- While using Redis with cluster mode enabled, you cannot manually promote any of the replica nodes to primary
- All the nodes in a Redis cluster must reside in the same region

**Sai**
- If you have no replicas and a node fails, you experience no loss of data when using Redis with cluster mode enabled
- You can scale write capacity for Redis by adding replica nodes
- While using Redis with cluster mode enabled, asynchronous replication mechanisms are used to keep the read replicas synchronized with the primary. If cluster mode is disabled, the replication mechanism is done synchronously

**Giải thích**

Nếu dùng Redis ở Cluster mode enable thì không thể thủ công tạo bất kỳ replica node nào với Primary.

Tất cả nodes trong 1 Redis cluter phải đặt trong 1 region.

## Câu 34: Bạn có 1 web application được host trên EC2 làm lệnh GET và PUT requests cho objects được lưu trong S3 đang dùng SDK cho PHP. Khi team bảo mật hoàn tất việc final review cho application của bạn cho vấn đề lỗ hổng (vulnerabilities), họ đã nhận thấy ứng dụng của bạn đã hardcode IAM access key và secret key để truy cập vào các AWS Services. Họ khuyên bạn nên tận dụng thêm secure setup, cái mà có thể sử dụng credentials tạm thời.

Lựa chọn nào sau đây có thể được sử dụng để giải quyết trường hợp này ? 

**Đúng**
- Use an IAM Instance Role

**Sai**
- Hardcode the credentials in the application code
- Use the SSM parameter store
- Use environment variables

**Giải thích**

Dùng instance role bởi vì EC2 đang request tới S3, vì vậy cần cấp quyền cho EC2 request tới S3 bucket. Dùng instance role thay vì Access key và Secret key.

## Câu 35: 1 Team .NET làm việc với nhiều ASP.NET web applications sử dụng EC2 instances để host chúng trên IIS. Deployment process cần được cấu hình cho nhiều versions của ứng dụng có thể chạy trên Elastic Beanstalk. 1 version sẽ được sử dụng cho development, testing, và các phiên bản các cho việc load testing. 

Phương thức nào sau đây bạn sẽ khuyên dùng cho trường hợp này ? 

**Đúng**
- Define a dev environment with a single instance and a 'load test' environment that has settings close to production environment

**Sai**
- Use only one Beanstalk environment and perform configuration changes using an Ansible script
- Create an Application Load Balancer to route based on hostname so you can pass on parameters to the development Elastic Beanstalk environment. Create a file in .ebextensions/ to know how to handle the traffic coming from the ALB
- You cannot have multiple development environments in Elastic Beanstalk, just one development and one production environment

**Giải thích**

Định nghĩa 1 dev env với 1 single instance và 1 'load test' env gần nhất với production env

## Câu 36: Bạn đã upload 1 ZIP file lên AWS Lambda chứa code files được viết bằng NodeJs. Khi cấu hình function được thực thi thì bạn nhận được output sau đây, 'Error: Memory Size: 10,240 MB Max Memory Used'.

Cách giải thích nào sau đây cho vấn đề này ? 

**Đúng**
- Your Lambda function ran out of RAM

**Sai**
- Your zip file is corrupt
- You have uploaded a zip file larger than 50 MB to AWS Lambda
- The uncompressed zip file exceeds AWS Lambda limits

**Giải thích**



## Câu 37: 1 người dùng có 1 IAM policy cũng như 1 SQS policy đucợ apply vào account của anh ấy. IAM policy gán cho tài khoản anh ấy quyền `ReceiveMessage` trên  `example_queue`, trong khi SQS policy cho tài khoản anh ấy quyền `SendMessage` trên cùng queue.

Đang cân nhắc quyền ở trên, lựa chọn nào sau đây là hợp lệ ? 

**Đúng**
- The user can send a `ReceiveMessage` request to `example_queue`, the IAM policy allows this action
- If you add a policy that denies the user access to all actions for the queue, the policy will override the other two policies and the user will not have access to `example_queue`

**Sai**
- Either of IAM policies or Amazon SQS policies should be used to grant permissions. Both cannot be used together
- If the user sends a `SendMessage` request to `example_queue`, the IAM policy will deny this action
- Adding only an IAM policy to deny the user of all actions on the queue is not enough. The SQS policy should also explicitly deny all action

**Giải thích**



## Câu 38: 1 senior Cloud Engineer thiết kế và triển khai 1 giải pháp phát hiện gian lận cho các công ty thẻ tín dụng đang xử lý hàng triệu giao dịch mỗi ngày. Ứng dụng trên Elastic Beanstalk gửi file tới S3 và sau đó gửi 1 message tới SQS chứa đường dẫn của file đã upload trên S3. Các kỹ sư muốn hoãn việc gửi bất kỳ message mới nào tới queue ít nhất 10 giây. 

Tính năng nào của SQS mà các kỹ sư nên tận dụng 

**Đúng**
- Use DelaySeconds parameter

**Sai**
- Enable LongPolling
- Implement application-side delay
- Use visibility timeout parameter

  **Giải thích**



## Câu 39: Bạn đang tyhieest kế 1 ứng dụng hiệu năng cao yêu cầu hàng triệu kết nối. Bạn có nhiều EC2 instances đang chạy Apache2 web servers và ứng dụng sẽ yêu cầu ghi lại source IP cua rnguowif dùng và source port không sử dụng X-Forwarded-For.

Lựa chọn nào sau đây sẽ đáp ứng điều bạn cần ? 

**Đúng**
- Network Load Balancer

**Sai**
- Elastic Load Balancer
- Classic Load Balancer
- Application Load Balancer

**Giải thích**



## Câu 40: Bạn đang được giao 1 là tech lead của 1 dự án mới là 1 web app xử lý đơn đặt hàng của khách hàng. Bạn muốn tích hợp event-driven processing cho bất kỳ dữ liệu nào bị thay đổi hoặc bị xóa và sử dụng cách tiếp cận serverless đang dùng Lambda cho xử lý event streaming.

Database nào sau đây mà bạn nên chọn ? 

**Đúng**
- DynamoDB

**Sai**
- Kinesis
- ElastiCache
- RDS

**Giải thích**



## Câu 41: 1 hệ thống quản lý đơn đặt hàng sử dụng 1 cron job để poll bất kỳ order mới nào. Bất kỳ lúc nào order mới được tạo, cron job gửi dữ liệu order này dưới dạng 1 message tới Message Queues để thuận tiện cho việc xử lý đơn đặt hàng ở downstream trong 1 cách đáng tin cậy. Để giarmn chi phí và cải thiện performance, công ty muốn di chuyển tính năng này lên AWS.

Giải pháp nào sau đây là tối ưu để đáp ứng yêu cầu ? 

**Đúng**
- Use Amazon Simple Notification Service (SNS) to push notifications when an order is created. Configure different Amazon Simple Queue Service (SQS) queues to receive these messages for downstream processing

**Sai**
- Configure different Amazon Simple Queue Service (SQS) queues to poll for new orders
- Use Amazon Simple Notification Service (SNS) to push notifications and use AWS Lambda functions to process the information received from SNS
- Use Amazon Simple Notification Service (SNS) to push notifications to Kinesis Data Firehose delivery streams for processing the data for downstream applications

**Giải thích**

Vì SNS cho phép ứng dụng gửi tin nhắn quan trọng tới nhiều Subscribers thông qua 1 cơ chế `Push`, loại bỏ việc check định kỳ hoặc `poll` để cập nhật dữ liệu. 

## Câu 42: Là 1 Full-stack Web Dev, bạn đang có liên quan đến mọi khía cạnh của platform của công ty từ phát triển PHP và JS tới cấu hình NoSQL DB bằng AWS DynamoDB. Bạn không lo lắng về việc nhận dữ liệu lỗi thời từ DB và cần thực hiện 16 lần Read consistencies/s, mỗi lần có kích thước 12KB.

Bạn cần bao nhiêu RCUs

**Đúng**
- 24

**Sai**
- 12
- 192
- 48

**Giải thích**

1 RCU = 1 strongly consistency read/s cho item <= 4KB. 1RCU = 2 eventually consistency/s cho item <= 4KB

DynamoDB làm tròn lên theo bội số của 4KB. 12KB/4KB = 3 RCUs cho mỗi strongly consistency read. Với eventually consistency read: 3/2 = 1.5 RCUs cho mỗi item.

Số lần đọc là 16, RCUs/item = 1.5 => total RCUs = 16 x 1.5 = 24 RCUs

## Câu 43: 1 Dev muốn 1 khả năng liền mạch để trở về phiên bản cũ hơn của Lambda Functions đã được deploy. 

Giải pháp nào sau đây cung cấp ít chi phí vận hành nhất ? 

**Đúng**
- Use a Lambda function alias that can point to the different versions

**Sai**
- Use CodeDeploy to configure blue/green deployments for the different Lambda function versions
- Use Lambda function layers that can point to the different versions
- Use a Route 53 weighted policy that can point to the different Lambda function versions

**Giải thích**



## Câu 44: 1 dev đã triển khai 1 lambda function để push data vào RDS MySQL DB cùng với python code sau:  
```
def handler(event, context):
    mysql = mysqlclient.connect()
    data = event['data']
    mysql.execute(f"INSERT INTO foo (bar) VALUES (${data});")
    mysql.close()
    return
```
Trong lần chạy code đầu tiên, Lambda function tốn 2 giây để thực thi. Từ lần thứ 2 trở đi, Lambda function tốn 1.9 giây để thực thi

Giải pháp nào để cải thiện execution time của Lambda functions 

**Đúng**
- Move the database connection out of the handler

**Sai**
- Change the runtime to Node.js
- Upgrade the MySQL instance type
- Increase the Lambda function RAM

**Giải thích**



## Câu 45: 1 công ty muốn di chuyển Code của ứng dụng đang tồn tại từ 1 GitHub repo lên AWS CodeCommit. Là 1 AWS DVA, lựa chọn nào sau đây mà bạn sẽ khuyên cho việc di chuyển repo được clone qua CodeCommit thông qua HTTPS ? 

**Đúng**
- Use Git credentials generated from IAM

**Sai**
- Use IAM Multi-Factor authentication
- Use authentication offered by GitHub secure tokens
- Use IAM user secret access key and access key ID

**Giải thích**

Đây là giải pháp được khuyến nghị cho hầu hết case. IAM user tạo ra 1 cặp username/password tĩnh dành riêng cho Git. Dễ thiết lập và sử dụng nhất. Hoạt động với mọi công cụ hỗ trợ Git. 

## Câu 46: Bạn làm việc là 1 Dev đang hợp tác với Nhà Nước trên AWS gov cloud. Ứng dụng của bạn sử dụng SQS cho message queue service. Dẫn tới có nguy cơ bị Hack, vấn đề bảo mật trở nên nghiêm ngặt và yêu cầu bạn lưu dữ liệu trong queue có khả năng bảo mật. 

Các bước nào sau đây mà bạn có thể làm để đáp ứng yêu cầu mà không làm thay đổi code đang có ? 

**Đúng**
- Enable SQS KMS encryption

**Sai**
- Use Client side encryption
- Use the SSL endpoint
- Use Secrets Manager

**Giải thích**



## Câu 47: Đối với ứng dụng lưu trữ thông tin sức khỏe cá nhân ( PHI ) trong 1 RDS for MySQL DB instance được mã hóa, 1 dev muốn cải thiện performance của nó bằng cách cache các dữ liệu được truy cập thường xuyên và thêm khả năng sắp xếp hoặc xếp hạng dataset được cache. 

Cách tiếp cận nào sau đây là tốt nhất để đáp ứng yêu cầu này để chứa PHI được mã hóa mọi lúc ? 

**Đúng**
- Store the frequently accessed data in an Amazon ElastiCache for Redis instance with encryption enabled for data in transit and at rest

**Sai**
- Migrate the frequently accessed data to DynamoDB Accelerator (DAX) that has encryption enabled for data in transit and at rest
- Migrate the frequently accessed data to an EC2 Instance Store that has encryption enabled for data in transit and at rest
- Store the frequently accessed data in an Amazon ElastiCache for Memcached instance with encryption enabled for data in transit and at rest

**Giải thích**


## Câu 48: 1 công ty an ninh mạng đang pushlish nhật ký dữ liệu quan trọng vào 1 log group trong AWS CloudWatch Logs, cái mà được tạo 3 tháng trước. Công ty phải mã hóa dữ liệu log đang dùng 1 AWS KMS customer master key ( CMK ), vì vậy bất kỳ dữ liệu nào sau này có thể được mã hóa để đáp ứng hướng dẫn bảo mật của công ty. 

Công ty có thể sử dụng giải pháp nào cho trường hợp này ? 

**Đúng**
- Use the AWS CLI `associate-kms-key` command and specify the KMS key ARN

**Sai**
- Use the AWS CLI `describe-log-groups` command and specify the KMS key ARN
- Enable the encrypt feature on the log group via the CloudWatch Logs console
- Use the AWS CLI `create-log-group` command and specify the KMS key ARN

**Giải thích**

## Câu 49: 1 công ty có vài LInux EC2 instances tạo nhiều log files cái mà cần cho việc phân tích cho yêu cầu tuân thủ và bảo mật. Công ty muốn sử dụng KDS để phân tích các dữ liệu log này. 

Cách nào sau đây tối ưu nhất cho việc gửi dữ liệu log từ EC2 lên KDS

**Đúng**
- Install and configure Kinesis Agent on each of the instances

**Sai**
- Install AWS SDK on each of the instances and configure it to send the necessary files to Kinesis Data Streams
- Run cron job on each of the instances to collect log data and send it to Kinesis Data Streams
- Use Kinesis Producer Library (KPL) to collect and ingest data from each EC2 instance

**Giải thích**



## Câu 50: Bạn đang lưu trữ video file của bạn trong 1 S3 bucket riêng biệt, so với S3 bucket đang host static website của bạn. Khi truy cập video URLs trực tiế thì người dùng có thể xem video trên browser, nhưng họ không thể xem videos trong khi tới website chính. 

Lý do gốc rễ của vấn đề này là gì ? 

**Đúng**
- Enable CORS

**Sai**
- Disable Server-Side Encryption
- Amend the IAM policy
- Change the bucket policy

**Giải thích**

## Câu 51: Công ty bạn đang trong quá trình xây dựng văn hóa DevOps và đang di chuyển tất cả tài nguyên On-Prem lên Cloud sử dụng serverless architecture và tự động deployments. Bạn đã tạo 1 CloudFormation template trong YAML, nó tạo 1 Lambda function để pull HTML file từ GitHub và lưu chúng qua 1 S3 bucket mà bạn chỉ định.

AWS CLI commands nào sau đây mà bạn có thể sử dụng để upload AWS Lambda function và AWS CloudFormation templates lên AWS ? 

**Đúng**
- cloudformation package and cloudformation deploy

**Sai**
- cloudformation zip and cloudformation upload
- cloudformation package and cloudformation upload
- cloudformation zip and cloudformation deploy

**Giải thích**

## Câu 52: 1 công ty IT đang dùng CloudFormation để quản lý IT infra của họ. Nó đã tạo 1 template để cung cấp 1 stacik cùng với 1 VPC và 1 subnet. Giá trị Output của subnet được dùng cho Stack khác. 

Là 1 DVA, Lựa chọn nào mà bạn sẽ đề xuất để cung cấp thông tin cho stack khác ? 

**Đúng**
- Use 'Export' field in the Output section of the stack's template

**Sai**
- Use 'Expose' field in the Output section of the stack's template
- Use Fn::ImportValue
- Use Fn::Transform

**Giải thích**

- B1: Export giá trị từ stack A
- B2: Import giá trị vào Stack B bằng Fn::ImportValue

## Câu 53: 1 website server static content từ 1 S3 bucket và dynamic content từ 1 ALB. Người dùng cơ bản trên toàn thế giới và latency nên tối thiểu cho trải nhiệm người dùng tốt hơn. 

Công nghệ/ Service nào có thể giúp truy cập static and dynamic content trong khi vẫn giữ latency thấp ? 

**Đúng**
- Configure CloudFront with multiple origins to serve both static and dynamic content at low latency to global users

**Sai**
- Use Global Accelerator to transparently switch between S3 bucket and load balancer for different data needs
- Use CloudFront's Origin Groups to group both static and dynamic requests into one request for further processing
- Use CloudFront's Lambda@Edge feature to server data from S3 buckets and load balancer programmatically on-the-fly

**Giải thích**

## Câu 54: 1 công ty dịch vụ tài chính với hơn 10000 nhân viên đã thuê bạn là 1 Senior Dev mới. Caching ban đầu đã được enable để giảm số call tới API endpoints và cải thiện latency của request cho API Gateway của công ty. 

Cho mục đích test, bạn muốn vô hiệu hóa caching cho API Clients để lấy dữ liệu gần nhất. Bạn nên làm cái nào sau đây ? 

**Đúng**
- Using the Header Cache-Control: max-age=0

**Sai**
- Using the request parameter ?cache-control-max-age=0
- Using the Header Bypass-Cache=1
- Use the Request parameter: ?bypass_cache=1

**Giải thích**

## Câu 55: Bạn đã cấu hình 1 NACL và 1 Security Grou cho Load Balancer và EC2 instances để cho phé inbound traffic trên port 80. Tuy nhiên, người dùng vẫn có thể kết nối tới website sau khi lanch.

Cấu hình nào sau đây được yêu cầu để làm cho website có thể được truy cập cho tất cả người dùng trên internet ? 

**Đúng**
- Add a rule to the Network ACLs to allow outbound traffic on ports 1024 - 65535

**Sai**
- Add a rule to the Network ACLs to allow outbound traffic on ports 1025 - 5000
- Add a rule to the Network ACLs to allow outbound traffic on ports 32768 - 61000
- Add a rule to the Security Group allowing outbound traffic on port 80

**Giải thích**

## Câu 56: AWS CodeDeploy của bạn triển khai trên T2 instances đã thành công. Phiên bản mới của ứng dụng API calls tới S3 tuy nhiên ứng dụng không hoạt động như mong muốn do lỗi xác thực và bạn đã được giao sửa vấn đề này. Lựa chọn nào sau đây bạn nên chọn ? 

**Đúng**
- Fix the IAM permissions for the EC2 instance role

**Sai**
- Fix the IAM permissions for the CodeDeploy service role
- Enable CodeDeploy Proxy
- Make the S3 bucket public

**Giải thích**



## Câu 57: Xem xét các IAM policy sau
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "s3:*",
      "Resource": "arn:aws:s3:::EXAMPLE-BUCKET/private*"
    },
    {
      "Effect": "Allow",
      "Action": ["s3:PutObject", "s3:GetObject"]
      "Resource": "arn:aws:s3:::EXAMPLE-BUCKET/*",
    }
  ]
}
```
Statements nào sau đây là hợp lệ cho policy ở trên ? 

**Đúng**
- The policy provides PutObject and GetObject access to all objects in the EXAMPLE-BUCKET bucket except the objects that start with `private`

**Sai**
- The policy provides PutObject and GetObject access to all buckets except the EXAMPLE-BUCKET/private bucket
- The policy denies PutObject and GetObject access to all buckets except the EXAMPLE-BUCKET/private bucket
- The policy provides PutObject and GetObject access to all objects in the EXAMPLE-BUCKET bucket as well as provides access to all s3 actions on objects starting with private in the EXAMPLE-BUCKET bucket

**Giải thích**

## Câu 58: Team dev của bạn sử dụng AWS SDK cho Java trên 1 Web app dùng uploads files tới nhiều S3 bucket sử dụng cơ chế mã hóa SSE-KMS. Các dev báo cáo nhận lỗi về Permission khi cố gắng push objects thông qua HTTP. Headers nào sau đây họ nên bao gồm trong request ?  

**Đúng**
- 'x-amz-server-side-encryption': 'aws:kms'

**Sai**
- 'x-amz-server-side-encryption': 'SSE-S3'
- 'x-amz-server-side-encryption': 'SSE-KMS'
- 'x-amz-server-side-encryption': 'AES256'

**Giải thích**



## Câu 59: Công ty của bạn tận dụng CloudFront để cung cấp nội dung internet tới khách hàng với low latency. Ngoài việc latency, bảo mật cũng là một mối quan ngại khác và bạn đang tìm cách giúp trong việc thiết lập kết nối đầu cuối sử dụng HTTPS để bảo vệ nội dung.

Lựa chọn nào là khả dụng cho HTTPS trong CloudFront ? 

**Đúng**
- Between clients and CloudFront as well as between CloudFront and backend

**Sai**
- Between clients and CloudFront only
- Neither between clients and CloudFront nor between CloudFront and backend
- Between CloudFront and backend only

**Giải thích**



## Câu 60: 1 công ty đã phát triển 1 dịch vụ dựa vào application cho cộng đồng để đặt các chuyến vận tải trong cộng đồng địa phương. Nền tảng đang chạy trên EC2 instance và sử dụng RDS cho lưu trữ dữ liệu vận tải. 1 tính năng mới được yêu cầu nơi mà sẽ nhận email từ các khách hàng gắn cùng file PDF nhận được từ S3. 

Lựa chọn nào sau đây sẽ cung cấp EC2 instance với các permission đúng để upload file lên S3 và tạo ra S3 Signed URL ? 

**Đúng**
- Create an IAM Role for EC2

**Sai**
- Run `aws configure` on the EC2 instance
- EC2 User Data
- CloudFormation

**Giải thích**

EC2 muốn truy cập S3 thì cần Permission như GetObject,... Vì cần quyền truy cập nên phải cấu hình Role cho EC2 instance là cách an toàn nhất để EC2 có thể thực hiện các tác vụ với S3 bucket. 

## Câu 61: Sau khi review AWS Bill hàng tháng của bạn, bạn nhận thấy chi phí sử dụng SQS tăng bất ngờ sau khi tạo SQS Queue mới, tuy nhiên bạn biết queue của bạn không có nhiều traffic và nhận các response trống.

Hành động nào sau đây bạn nên làm ? 

**Đúng**
- Use LongPolling

**Sai**
- Increase the VisibilityTimeout
- Use a FIFO queue
- Decrease DelaySeconds

**Giải thích**



## Câu 62: 1 công ty bảo trì 1 highly available application, ứng dụng nhận HTTPS traffic từ mobile device và web browsers. Các dev chính muốn setup Load Balancer routing để route trafic từ web servers tới smart.com/api và từ mobile devices tới smart.com/mobile. 1 dev khuyên rằng khuyến nghị trước đó không còn cần thiết nữa và requests thay vì đó nên được gửi tới api.smart.com và mobile.smart.com.

Lựa chọn routing nào sau đây cho trường hợp này ? 

**Đúng**
- Host based
- Path based

**Sai**
- Web browser version
- Client IP
- Cookie value

**Giải thích**



## Câu 63: Các DevOps đang phát triển 1 hệ thống xử lý đơn đặt hàng nơi maf thông báo gửi tới 1 phòng ban bất kỳ khi nào 1 đơn đặt hàng được đặt cho 1 sản phẩm. Hệ thống cũng push thông báo giống hệt của 1 đơn đặt hàng mới tới 1 module xử lý cho phép EC2 instances xử lý hoàn thành đơn đặt hàng. Trong trường hợp xử lý lỗi, messages nên được cho phép để xử lý lại wor state sau. Hệ thống xử lý đơn đặt hàng nên có khả năng scale rõ ràng mà không cần bất kỳ việc cung cấp resource thủ công hoặc chương trình. 

Giải pháp nào sau đây có thể được sử dụng để giải quyết trường hợp này với cách chi phí hiệu quả nhất ? 

**Đúng**
- SNS + SQS

**Sai**
- SQS + SES
- SNS + Kinesis
- SNS + Lambda

**Giải thích**



## Câu 64: Công ty của bạn quản lý hàng trăm EC2 instances chạy Linux OS. Các instances được cấu hình trên nhiều AZs trong `eu-west-3`. Quản lý của bạn đã yêu cầu sưu tập các metric của system memory trên tất cả EC2 instances sử dụng 1 script.

Giải pháp nào sau đây sẽ giúp bạn sưu tập dữ liệu này ? 

**Đúng**
- Use a cron job on the instances that pushes the EC2 RAM statistics as a Custom metric into CloudWatch

**Sai**
- Extract RAM statistics from the standard CloudWatch metrics for EC2 instances
- Extract RAM statistics using the instance metadata
- Extract RAM statistics using X-Ray

**Giải thích**



## Câu 65: Bạn đang bắt đầu cho 1 sự kiện viêt JS bằng Alexa skill. Khi bạn test lệnh bằng giọng, bạn tìm thấy một số intent không được gọi như mong muốn và bạn đang gặp khó khăn trong việc tìm hiểu nguyên nhân. Bạn đã đính kèm đoạn code sau `console.log(JSON.stringify(this.event))` hi vọng lấy chi tiết về requests của bạn trong AWS Alexa skill.

Bạn muốn logs được lưu trữ trong S3 bucket tên là `MyAlexaLog`. Cách để đạt được điều này là thế nào ? 

**Đúng**
- Use CloudWatch integration feature with S3

**Sai**
- Use CloudWatch integration feature with Glue
- Use CloudWatch integration feature with Lambda
- Use CloudWatch integration feature with Kinesis

**Giải thích**
