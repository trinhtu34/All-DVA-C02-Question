## Câu 1: Bạn đang làm việc cho công ty vận chuyển, công ty tự động tạo ECS clusters với 1 ASG sử dụng 1 CloudFormation template chấp nhận cluster name là Parameters của nó. Ban đầu, bạn launch template với giá trị input là `MainCluster`, cái mà được deploy 5 instance trên 2 AZs. Lần thứ 2, bạn launch template cùng với 1 giá trị input là `SecondCluster`. Tuy nhiên, instances được tạo ở lần 2 chạy thì cũng launch trong `MainCluster` kể cả sau khi chỉ định Cluster Name khác.

Gốc rễ của vấn đề là gì ? 

**Đúng**
- The cluster name Parameter has not been updated in the file /etc/ecs/ecs.config during bootstrap

**Sai**
- The security groups on the EC2 instance are pointing to the wrong ECS cluster
- The ECS agent Docker image must be re-built to connect to the other clusters
- The EC2 instance is missing IAM permissions to join the other clusters

**Giải thích**

Cluster name Parameters không được update trong /etc/ecs/ecs.config. Để cấu hình đúng thì cần 
sử dụng echo để copy variable vào configuration:
```bash
echo "ECS_CLUSTER=SecondCluster" >> /etc/ecs/ecs.config
```

## Câu 2: 1 Dev muốn tích hợp file upload được chỉ định bởi user và tính năng tải xuống trong 1 ứng dụng sử dụng Cognito user pools và Cognito identity pools để truy cập bảo mật vào S3. Dev cũng muốn đảm bảo chỉ có user được xác thực mới có thể truy cập vào các file họ sở hữu và file được lưu và truy xuất một cách an toàn. File có kích thước từ 5 KB tới 500MB.

Giải pháp gì là gợi ý hiệu quả nhất ? 

**Đúng**
- Leverage an IAM policy with the Amazon Cognito identity prefix to allow users access to just their own objects in Amazon S3

**Sai**
- Use CloudFront Lambda@Edge to validate that the given file is uploaded to S3 and downloaded from S3 only by the authorized user
- Integrate Amazon API Gateway with a Lambda function that validates that the given file is uploaded to S3 and downloaded from S3 only by the authorized user
- Use S3 Event Notifications to trigger a Lambda function that validates that the given file is uploaded and downloaded only by the authorized user

**Giải thích**

Cognito identity pool cung cấp AWS credentials duy nhất tạm thời gắn với IAM Role và Policy động theo identityID

Cognito identity pools hỗ trợ các identity providers sau:
- Public providers: Login with Amazon (identity pools), Facebook (identity pools), Google (identity pools), Sign in with Apple (identity pools).
- Amazon Cognito user pools, OpenID Connect providers (identity pools), SAML identity providers (identity pools), Developer authenticated identities (identity pools)

Bạn có thể tạo 1 identity-based policy có thể cho phép Cognito users truy cập vào objects trong 1 S3 bucket được chỉ định. Policy này cho phép chỉ truy cập object cùng với 1 name bao gồm Cognito, tên của application.

## Câu 3: 1 công ty dịch vụ tài chính hàng đầu cung cấp dịch vụ tổng hợp dữ liệu của Wall Street. Bill của khách hàng tính tiền dựa vào mỗi đơn vị clickstream được cung cấp cho khách hàng. Là 1 công ty hoạt động trong lĩnh vực được quản lý chặt chẽ, nó cần phải có dữ liệu clickstream được sắp xếp theo cùng thứ tự cho việc audit trong vòng 7 ngày.

Là 1 DVA, AWS Service nào mà bạn nghĩ là cung cấp khả năng vận hành quy trình tính bill và autdit trên dữ liệu clickstrema được sắp xếp theo cùng thứ tự

**Đúng**
- AWS Kinesis Data Streams

**Sai**
- Amazon SQS
- AWS Kinesis Data Analytics
- AWS Kinesis Data Firehose

**Giải thích**

KDF có thể:
- Nhận dữ liệu real-time
- Giữ thứ tự dữ liệu
- Nhiều application đọc song song
- Đọc lại dữ liệu cũ ( Replay )

## Câu 4: là 1 phần của kế hoạch on-boarch, nhân viên tại 1 công ty IT cần upload profile photos vào 1 private S3 bucket. Công ty muốn build 1 in-house web application được host trên 1 EC2 instance nên hiển thị profile photos 1 cách bảo mật khi nhân viên điểm danh

Là 1 DVA, Giải pháp nào sau đây bạn sẽ đề xuất để giải quyết trường hợp này ? 

**Đúng**
- Save the S3 key for each user's profile photo in a DynamoDB table and use a lambda function to dynamically generate a pre-signed URL. Reference this URL for display via the web application

**Sai**
- Keep each user's profile image encoded in base64 format in a DynamoDB table and reference it from the application for display
- Keep each user's profile image encoded in base64 format in an RDS table and reference it from the application for display
- Make the S3 bucket public so that the application can reference the image URL for display

**Giải thích**

-Quy trình: Client upload vào S3 -> DynamoDB lưu link, userid
-Khi điểm danh: Web app gọi Lambda -> Lambda dùng IAM Role tạo Pre-signed URL 

## Câu 5: 1 công ty áp dụng các phương pháp triển khai collaborative. Quản lý kỹ thuật muốn cô lập nỗ lực deployment bằng cách giả lập các API component được sở hữu bởi nhiều dev teams. 

API Integration type nào là tốt nhất cho yêu cầu này 

**Đúng**
- MOCK

**Sai**
- HTTP
- HTTP_PROXY
- AWS_PROXY

**Giải thích**

## Câu 6: 1 công ty truyền thông sử dụng SQS queue để quản lý transactions của  họ. Cùng với các thay đổi cần thiết của doanh nghiệp, payload size của messages đang tăng. Team lead của dự án lo lắng về 256KB message mà SQS có.

Cần làm gì để queue có thể chấp nhận các messages lớn hơn. 

**Đúng**
- Use the SQS Extended Client

**Sai**
- Use the MultiPart API
- Get a service limit increase from AWS
- Use gzip compression

**Giải thích**

`SQS Extended Client` đây là pattern chuẩn của AWS.

## Câu 7: 1 intern tại 1 công ty IT  đang bắt đầu với AWS và muốn hiểu Amazon S3 bucket access policy sau, bạn có giúp anh ấy xác định đây là policy cho cái gì không.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ListAllS3Buckets",
            "Effect": "Allow",
            "Action": ["s3:ListAllMyBuckets"],
            "Resource": "arn:aws:s3:::*"
        },
        {
            "Sid": "AllowBucketLevelActions",
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetBucketLocation"
            ],
            "Resource": "arn:aws:s3:::*"
        },
        {
            "Sid": "AllowBucketObjectActions",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:PutObjectAcl",
                "s3:GetObject",
                "s3:GetObjectAcl",
                "s3:DeleteObject"
            ],
            "Resource": "arn:aws:s3:::*/*"
        },
        {
            "Sid": "RequireMFAForProductionBucket",
            "Effect": "Deny",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::Production/*",
                "arn:aws:s3:::Production"
            ],
            "Condition": {
                "NumericGreaterThanIfExists": {"aws:MultiFactorAuthAge": "1800"}
            }
        }
    ]
}
```

**Đúng**
- Allows full S3 access, but explicitly denies access to the Production bucket if the user has not signed in using MFA within the last thirty minutes

**Sai**
- Allows full S3 access to an Amazon Cognito user, but explicitly denies access to the Production bucket if the Cognito user is not authenticated
- Allows IAM users to access their own home directory in Amazon S3, programmatically and in the console
- Allows a user to manage a single Amazon S3 bucket and denies every other AWS action and resource if the user is not signed in using MFA within last thirty minutes

**Giải thích**

Bucket policy này có ý nghĩa là: cho phép full S3 access, nhưng sẽ bị deny access lên Production bucket nếu người dùng không đăng nhập sử dụng MFA trong vòng 30 phút

## Câu 8: 1 công ty lớn lưu trữ static data assets trên S3 buckets. Mỗi dòng dịch vụ của công ty có AWS account. Đối với doanh nghiệp, phòng tài chính cần truy cập vào dữ liệu trong S3 bucket của phòng HR.

Lựa chọn nào là không khả thi cho việc truy cập S3 bucket object trên nhiều account 

**Đúng**
- Use IAM roles and resource-based policies delegate access across accounts within different partitions via programmatic access only

**Sai**
- Use Resource-based policies and AWS Identity and Access Management (IAM) policies for programmatic-only access to S3 bucket objects
- Use Cross-account IAM roles for programmatic and console access to S3 bucket objects
- Use Access Control List (ACL) and IAM policies for programmatic-only access to S3 bucket objects

**Giải thích**

Cách này không khả thi vì vấn đề nằm ở Different Partitions. AWS có các partitions khác nhau như: aws, aws-cn, aws-us-gov. IAM Roles + resource-based policy không thể delegate access xuyên partitions, chỉ hoạt động trong cùng partitions 

Các cách hợp lệ:
- Resource-based policies + IAM policies (programmatic-only): Cách phổ biến, dùng cho SDK - CLI - App
- Cross-Account IAM roles (programatic + console): AWS highly recommend, dùng cho Console - CLI - SDK
- ACL + IAM policies (programmatic-only): Dùng được nhưng không nên

## Câu 9: 1 Dev từ team của bạn đã cấu hình Load Balancer để route traffic như nhau giữa các instances hoặc các AZs. Tuy nhiên, ELB route nhiều traffic tới 1 instance nhiều hơn ccs instance ở AZs khác.

Tại sao điều này xảy ra và cách sửa nó thế nào ? 

**Đúng**
- Instances of a specific capacity type aren’t equally distributed across Availability Zones
- Sticky sessions are enabled for the load balancer

**Sai**
- For Application Load Balancers, cross-zone load balancing is disabled by default
- There could be short-lived TCP connections between clients and instances
- After you disable an Availability Zone, the targets in that Availability Zone remain registered with the load balancer, thereby receiving random bursts of traffic

**Giải thích**

1. ASG không đảm bảo số instance bằng nhau ở mỗi AZs. Dù ELB có chia đều theo AZ thì sẽ có AZ nhận nhiều hơn traffic hơn vì có nhiều Capacity hơn. Cách khắc phục: bật Auto Scaling Balanced AZ hoặc kiểm tra lại ASG distribution
2. Sticky session là 1 client luôn được ELB gửi tới cùng 1 Backend instance

## Câu 10: 1 team đang kiểm tra khả năng tồn tại của 1 Step Functions cho việc tạo 1 banking workflow cho việc chấp thuận khoản vay. Web app cũng sẽ có chấp thuận của con người là 1 bước trong workflow. 

Là 1 DVA, bạn sẽ xác định key characteristics nào sau đây là đặc điểm của Step Functions

**Đúng**
- You should use Express Workflows for workloads with high event rates and short duration
- Standard Workflows on AWS Step Functions are suitable for long-running, durable, and auditable workflows that can also support any human approval steps

**Sai**
- Express Workflows have a maximum duration of five minutes and Standard workflows have a maximum duration of 180 days or 6 months
- Both Standard and Express Workflows support all service integrations, activities, and design patterns
- Standard Workflows on AWS Step Functions are suitable for long-running, durable, and auditable workflows that do not support any human approval steps

**Giải thích**

Standard workflows trên AWS Step Functions phù hợp cho long-running, durable, auditable workflow cũng hỗ trợ human approval steps. 

Nên sử dụng Express workflows cho các workloads high event rate và short duration. Express workflows hỗ trợ event rates hơn 100.000/s

## Câu 11: 1 công ty IT đã migrate tới 1 Serverless Application stack trên AWS cùng với compute layer được implement thông qua Lambda functions. Quản lý team kỹ sư muốn troubleshoot bất kỳ lỗi nào trong Lambda functions.

Là 1 DVA, Giải pháp nào sau đây bạn sẽ đề xuất cho trường hợp này ?

**Đúng**
- The developers should insert logging statements in the Lambda function code which are then available via CloudWatch logs

**Sai**
- Use Amazon EventBridge to identify and notify any failures in the Lambda code
- Use CodeCommit to identify and notify any failures in the Lambda code
- Use CodeDeploy to identify and notify any failures in the Lambda code

**Giải thích**

Khi bạn invoke 1 Lambda functions, 2 loại error có thể xảy ra. Invocation error xảy ra khi gọi request bị từ chối trước khi functions nhận nó. Function errors xảy ra khi lỗi code của bạn trả về lỗi. 

## Câu 12: 1 dev team đã deploy 1 REST API trong AWS API Gateway tới 2 stages khác nhau, 1 test stage và 1 prod stage. Test stage được sử dụng khi test build và prod stage dùng khi stable build. Sau khi updates pass được test stage, team mong muốn có thể chuyển từ test stage qua prod stage

Giải pháp nào sau đây là tối ưu cho trường hợp này ?

**Đúng**
- Update stage variable value from the stage name of test to that of prod

**Sai**
- Deploy the API without choosing a stage. This way, the working deployment will be updated in all stages
- Delete the existing prod stage. Create a new stage with the same name (prod) and deploy the tested version on this stage
- API performance is optimized in a different way for prod environments. Hence, promoting test to prod is not correct. The promotion should be done by redeploying the API to the prod stage

**Giải thích**

Sau khi tạo API của bạn, bạn phải deploy nó để nó có thể được người dùng của bạn gọi. Để deploy 1 API, bạn tạo 1 API deployment và associate nó vào 1 stage. 1 Stage là 1 tham chiếu logic tới 1 lifecycle state của API của bạn ví dụ như dev, prod, beta, v1, v2,...API Stage được xác định bởi API ID và stage name. Chúng bao gồm URL bạn invoke API. Mỗi stage là 1 thanm chiếu được chỉ định tới 1 deployment của API và nó available cho client application để call. 

## Câu 13: Công ty của bạn có 1 hợp đồng 3 năm với nhà cung cấp chăm sóc sức khỏe. Hợp đồng quy định các bản backup của database hàng tháng phải được lưu giữ trong suốt thời hạn hợp đồng vì mục đích tuân thủ. Hiện tại, giới hạn của backup retention được tự động, trên RDS, không đáp ứng được yêu cầu của bạn.

Giải pháp nào sau đây có thể giúp bạn đáp ứng yêu cầu 

**Đúng**
- Create a cron event in CloudWatch, which triggers an AWS Lambda function that triggers the database snapshot

**Sai**
- Enable RDS automatic backups
- Enable RDS Multi-AZ
- Enable RDS Read replica

**Giải thích**

Có nhiều cách để chạy periodic jobs ( công việc định kỳ ) trên AWS. EventBridge với Lambda là giải pháp đơn giản nhất. Để làm điều này, tạo 1 CloudWatch Rule và chọn "Scledule" là Event Source. Bạn có thể sử dụng 1 trong 2 cron expression hoặc cung cáp 1 tỷ lệ cố định chẳng hạn như mỗi 5 phút. Tiếp theo, chọn "Lambda Function" là Target. Lambda của bạn sẽ cần code cần thiết cho tính năng snapshot. 

## Câu 14: Team lead của bạn đã yêu cầu code review cho code của bạn ở Lambda functions. Code của bạn được viết bằng python và sử dụng S3 để upload logs lên 1 S3 bucket. Sau khi review, team lead đã gợi ý sử dụng lại execution context để cải thiện performance của Lambda. 

Hành động nào sau đây sẽ giúp bạn implement gợi ý này ? 

**Đúng**
- Move the Amazon S3 client initialization, out of your function handler

**Sai**
- Assign more RAM to the function
- Enable X-Ray integration
- Use environment variables to pass operational parameters
 
**Giải thích**

AWS best practice cho Lambda là đề xuất sử dụng execution context để cải thiện performance cho functions của bạn. Thêm SDK clients và database connection ở bên ngoài của xử lý function, và cache static assets local ở thư mục /tmp. Các lần invoke tiếp theo sẽ được sử lý trên cùng 1 instance của functions, functions của bạn có thẻ tái sử dụng lại tài nguyên. Điều này lưu lại time và cost. Để tránh rò rỉ dữ liệu giữa các lần invoke, đừng sử dụng execution context để lưu dữ liệu người dùng, event, hoặc các thông tin khác với mục đích bảo mật.

## Câu 15: 1 công ty thương mại điện tử có 1 workflow xử lý đơn đặt hàng với nhiều tasks có theerr làm song song cũng như các bước quyết định để dự báo cho xử lý đặt hàng thành công. Tất cả tasks được implement thông qua Lambda functions.

Giải pháp nào sau đây là tốt nhất để đáp ứng yêu cầu của doanh nghiệp ? 

**Đúng**
- Use AWS Step Functions state machines to orchestrate the workflow

**Sai**
- Use AWS Glue to orchestrate the workflow
- Use AWS Batch to orchestrate the workflow
- Use AWS Step Functions activities to orchestrate the workflow

**Giải thích**



## Câu 16: Team dev tại 1 công ty truyền thông đang làm việc trên 1 database được bảo mật

AWS database engine nào có thể được cấu hình với IAM Database Authentication 

**Đúng**
- RDS PostGreSQL
- RDS MySQL

**Sai**
- RDS Db2
- RDS SQL Server
- RDS Oracle

**Giải thích**



## Câu 17: 1 ứng dụng ngân hàng cần gửi cảnh báo và thông báo realtime dựa vào bất kỳ update nào từ Backend services. Công ty muốn tránh việc implement các cơ chế polling phức tạp cho Thông báo.

Loại API nào sau đây hỗ trợ bởi API Gateway là đúng 

**Đúng**
- WebSocket APIs

**Sai**
- REST or HTTP APIs
- HTTP APIs
- REST APIs

**Giải thích**



## Câu 18: Bạn đang chạy 1 Cloud file storage website với 1 internet-facing ALB, cái mà điều hướng request từ user thông qua internet tớ 10 EC2 instance. Ngườ dùng phàn nàn website của bạn luôn hỏi xác thực lại khi họ chuyển pages. Bạn bối rối bở vì hiện tượng này không xuất hiện trên local hoặc dev env của bạn. 

Lý do có thể là gì ? 

**Đúng**
- The Load Balancer does not have stickiness enabled

**Sai**
- The EC2 instances are logging out the users because the instances never have access to the client IPs because of the Load Balancer
- Application Load Balancer is in slow-start mode, which gives ALB a little more time to read and write session data
- The Load Balancer does not have TLS enabled

**Giải thích**

Nếu bị xác thực liên tục thì khả năng cao là do mỗi lần gửi traffic thì traffic lại vào 1 instance khác trong ALB, vì vậy bạn cần phải bật **Stickiness session**

## Câu 19: Là 1 Senior Dev, bạn quản lý 10 EC2 instance có read-heavy database request tới RDS PostgreSQL. Bạn cần làm cho kiến trúc này có khả năng phục hồi cho Disaster Recovery

Tính năng nào sau đây sẽ giúp bạn chuẩn bị cho Database Disaster Recovery ? 

**Đúng**
- Enable the automated backup feature of Amazon RDS in a multi-AZ deployment that creates backups in a single AWS Region
- Use cross-Region Read Replicas

**Sai**
- Enable the automated backup feature of Amazon RDS in a multi-AZ deployment that creates backups across multiple Regions
- Use RDS Provisioned IOPS (SSD) Storage in place of General Purpose (SSD) Storage
- Use database cloning feature of the RDS DB cluster

**Giải thích**

Chỉ tạo được backup trong Single AWS Region thôi. 

## Câu 20: Bạn đã migrate 1 on-premises SQL Server database lên RDS database được gắn 1 VPC và bên trong 1 private subnet. 
Ngoài ra, ứng dụng Java liên quan được host ở on-prem, đã được move tới Lambda function.

Bạn nên implement giải pháp nào sau đây để kết nối AWS Lambda function tới RDS instance của nó ? 

**Đúng**
- Configure Lambda to connect to VPC with private subnet and Security Group needed to access RDS

**Sai**
- Use Lambda layers to connect to the internet and RDS separately
- Use Environment variables to pass in the RDS connection string
- Configure lambda to connect to the public subnet that will give internet access and use Security Group to access RDS inside the private subnet

**Giải thích**



## Câu 21: Dev team tại 1 công ty muốn mã hóa 111GB đối tượng sử dụng AWS KMS. Giải pháp nào sau đây là tốt nhất ? 

**Đúng**
- Make a `GenerateDataKey` API call that returns a plaintext key and an encrypted copy of a data key. Use a plaintext key to encrypt the data

**Sai**
- Make a `GenerateDataKeyWithoutPlaintext` API call that returns an encrypted copy of a data key. Use an encrypted key to encrypt the data
- Make a `GenerateDataKeyWithPlaintext` API call that returns an encrypted copy of a data key. Use a plaintext key to encrypt the data
- Make an `Encrypt` API call to encrypt the plaintext data as ciphertext using a customer master key (CMK) with imported key material

**Giải thích**

Làm 1 cái `GenerateDataKey` API call trả về 1 plaintext key và 1 copy được mã hóa của 1 data key. Sử dụng 1 plaintext key để mã hóa dữ liệu.

## Câu 22: 1 tổ chức đang di chuyển tài nguyên on-prem lên Cloud. Source code sẽ được di chuyển tới AWS CodeCommit và AWS CodeBuild để sử dụng để đáp ứng source code sử dụng Apache Maven là 1 build tool. Tổ chức muốn build env nên cho phép scaling và running build song song. 

Tổ chức nên chọn lựa chọn nào sau đây cho yêu cầu của họ ? 

**Đúng**
- CodeBuild scales automatically, the organization does not have to do anything for scaling or for parallel builds

**Sai**
- Enable CodeBuild Auto Scaling
- Choose a high-performance instance type for your CodeBuild instances
- Run CodeBuild in an Auto Scaling group

**Giải thích**



## Câu 23: Kiến trúc Web app của bạn nhất quán trên nhiều EC2 instances vận hành sau 1 ELB với 1 ASG có desired capacity 5 EC2 instances. Bạn muốn tích hợp AWS CodeDeploy cho việc tự động deploy application. Deployment nên re-route traffic từ môi trường gốc của ứng dụng của bạn tới env mới.

Lựa chọn nào sau đây sẽ đáp ứng têu chuẩn deployment của bạn ? 

**Đúng**
- Opt for Blue/Green deployment

**Sai**
- Opt for In-place deployment
- Opt for Rolling deployment
- Opt for Immutable deployment

**Giải thích**



## Câu 24: bạn có 1 workflow xử lý các pulls code từ AWS CodeCommit và deploy lên EC2 instances được liên kết với các tag group ProdBuilders. Bạn muốn cấu hình instances để lưu trữ không nhiều hơn 2 phiên bản ứng dụng để tiết kiệm dung lượng ổ đĩa. 

Bạn sẽ cho phép implement lựa chọn nào sau đây ?

**Đúng**
- CodeDeploy Agent

**Sai**
- Integrate with AWS CodePipeline
- Have a load balancer in front of your instances
- AWS CloudWatch Log Agent

**Giải thích**

CodeDeploy agent là 1 software package, khi được tải xuống và cấu hình trên 1 instance, điều này cho phép nó được sử dụng trong việc triển khai CodeDeploy. CodeDeploy agent lưu trữ các phiên bản và log file trên instances. CodeDeploy agent dọn dẹp các artifacts để tiết kiệm dung lượng disk space. 

## Câu 25: 1 dev muốn cho phép X-Ray tracing trên 1 on-premises Linux server vận hành trên 1 custom application được truy cập thông qua API Gateway

Giải pháp gì là hiệu quả nhất cho yêu cầu cấu hình tối thiểu ?

**Đúng**
- Install and run the X-Ray daemon on the on-premises servers to capture and relay the data to the X-Ray service

**Sai**
- Install and run the X-Ray SDK on the on-premises servers to capture and relay the data to the X-Ray service
- Configure a Lambda function to analyze the incoming traffic data on the on-premises servers and then relay the X-Ray data to the X-Ray service using the PutTelemetryRecords API call
- Install and run the CloudWatch Unified Agent on the on-premises servers to capture and relay the X-Ray data to the X-Ray service using the PutTraceSegments API call

**Giải thích**

Tải và chạy X-Ray deamon trên on-premises servers vì X-Ray có 2 phần quan trọng X-Ray SDK - thêm code vào app , X-Ray deamon - gửi trace data lên AWS X-Ray 

Điểm mấu chốt của câu hỏi là giảm thiểu cấu hình => cài deamon thì chỉ cần cài và chạy service, không sửa code, api gateway đã tạo sẵn trace header sẵn. 

Không dùng SDK vì phải sửa code, build lại app

## Câu 26: AWS CloudFormation giúp mô hình hóa và cấp phát tất cả cloud infra resource cần thiết cho doanh nghiệp của bạn

Dịch vụ nào sau đây phụ thuộc vào CloudFormation để cấp phát resources ? 

**Đúng**
- AWS Serverless Application Model (AWS SAM)
- AWS Elastic Beanstalk

**Sai**
- AWS Lambda
- AWS Autoscaling
- AWS CodeBuild

**Giải thích**



## Câu 27: 1 công ty an ninh mạng đang vận hành 1 serverless backend với 1 vài compute-heavy workflows vận hành trên Lambda functions. Team dev đã nhận thấy 1 lag về performance sau khi phân tích performance metrics của Lambda functions.

Là 1 DVA, Lựa chọn nào sau đây mà bạn sẽ đề xuất là tốt nhất để giải quyết vấn đề compute-heavy workloads ? 

**Đúng**
- Increase the amount of memory available to the Lambda functions

**Sai**
- Use provisioned concurrency to account for the compute-heavy workflows
- Use reserved concurrency to account for the compute-heavy workflows
- Invoke the Lambda functions asynchronously to process the compute-heavy workflows

**Giải thích**

Compute-heavy thì phải tăng amount of memory available của Lambda

## Câu 28: Công ty của bạn sử dụng 1 ALB để điều hướng incoming end-user traffic tới ứng dụng được host trên EC2 instance. Ứng dụng ghi lại thông tin incoming request và lưu trên RDS chạy SQL Server DB engines.

Là 1 phần của việc tuân thủ rule mới, bạn cần ghi lại địa chỉ IP của client. Bạn sẽ đạt được điều này thế nào ?  

**Đúng**
- Use the header X-Forwarded-For

**Sai**
- You can get the Client IP addresses from server access logs
- You can get the Client IP addresses from Elastic Load Balancing logs
- Use the header X-Forwarded-From

**Giải thích**

Sử dụng header X-Forwarded-For - The X-Forwarded-For reqeust header giúp bạn xác định IP address của 1 client khi bạn sử dụng 1 HTTP hoặc HTTPS LB. Bởi vì LBs chặn traffic giữa client và servers, server access logs của bạn chỉ chứa địa chỉ IP của LB. Để thấy địa chỉ IP của Client, sử dụng X-Forwarded-For request header. 

## Câu 29: Team bạn bảo trì 1 public API Gateway được truy cập bởi client từ domain khác. Usage ổn định trong vài tháng qua nhưng gần đây đã tăng gấp đôi. Là 1 hệ quả, chi phí của bạn tăng và muốn tránh các domain không xác thực truy cập vào API của bạn

**Đúng**
- Restrict access by using CORS

**Sai**
- Use Mapping Templates
- Assign a Security Group to your API Gateway
- Use Account-level throttling

**Giải thích**

Muốn hạn chế truy cập từ các domain lạ thì dùng CORS

## Câu 30: 1 website thương mại điện tử của công ty đang ghi nhận hàng trăm và ngàn lượt truy cập vào Black Friday. Phòng marketing lo high volume của đơn đặt hàng sẽ dẫn tới quá tải cho hệ thống SQS. Công ty đã xin lời huyên của bạn để phòng ngừa tình trạng high volume.

Bạn sẽ đề xuất bước tiếp theo là gì ? 

**Đúng**
- Amazon SQS is highly scalable and does not need any intervention to handle the expected high volumes

**Sai**
- Convert the queue into FIFO ordered queue, since messages to the down system will be processed faster once they are ordered
- Pre-configure the SQS queue to increase the capacity when messages hit a certain threshold
- Enable auto-scaling in the SQS queue

**Giải thích**

SQS tận dụng khả năng Dynamically scale của AWS, dựa vào on demand. 

## Câu 31: Bạn là 1 dev đang làm việc với AWS CLI để tạo Lambda functions chưa env variables. Functions của bạn sẽ yêu cầu hơn 50 env variables bao gồm thông tin nhạy cảm của database table names.

Tổng số size/number của env variables bạn có thể tạo cho Lambda là gì ? 

**Đúng**
- The total size of all environment variables shouldn't exceed 4 KB. There is no limit on the number of variables

**Sai**
- The total size of all environment variables shouldn't exceed 4 KB. The maximum number of variables that can be created is 35
- The total size of all environment variables shouldn't exceed 8 KB. The maximum number of variables that can be created is 50
- The total size of all environment variables shouldn't exceed 8 KB. There is no limit on the number of variables

**Giải thích**

Không có giới hạn cho số env variables mà bạn có thể có trong Lambda functions, và tổng size của tất cả không vượt quá 4 KB.

## Câu 32: 1 công ty có hơn 100Tr thành viên trên toàn thế giới đang tham gia 125 triệu giờ xem trên TV shows và movies mỗi ngày. Công ty sử dụng AWS gần như cho tất cả computing và storage cần thiết, cái mà sử dụng hơn 10000 server instances trên AWS. Điều này dẫn tới 1 sự phức tạp và biến động networking environment rất lớn nơi mà ứng dụng liên tục giao tiếp bên trong AWS và trên khắp internet. Monitoring và Optimizing network là cực kỳ quan trọng cho công ty.

Công ty cần 1 giải pháp cho việc t hu thập và phân tích hàng TB dữ liệu real-time được tạo bởi network của nó hàng ngày dưới dạng flow logs. Công nghệ/dịch vụ nào mà công ty nên sử dụng để thu thập dữ liệu một cách tiết kiệm và linh hoạt để đưa trực tiếp dữ liệu này tới các hệ thống downstream khác ? 

**Đúng**
- Amazon Kinesis Data Streams

**Sai**
- AWS Glue
- Amazon Kinesis Firehose
- Amazon Simple Queue Service (SQS)

**Giải thích**

KDS là 1 service có khả năng scale và độ bền trong việc read-time data streaming lớn. KDS có thể liên tục ghi lại hàng BG data/s từ hàng trăm hàng ngàn sources chẳng hạn như clickstreams, database event streams, financial transactions, social media feeds.

## Câu 33: Để đáp ứng hướng dẫn tuân thủ, 1 công ty cần đảm bảo sao chép bất kỳ dữ liệu nào được lưu trong S3 buckets.

Các đặc điểm nào sau đây là chính xác trong khi cấu hình 1 S3 bucket cho sao chép ( replication ) ? 

**Đúng**
- Same-Region Replication (SRR) and Cross-Region Replication (CRR) can be configured at the S3 bucket level, a shared prefix level, or an object level using S3 object tags
- S3 lifecycle actions are not replicated with S3 replication

**Sai**
- Object tags cannot be replicated across AWS Regions using Cross-Region Replication
- Once replication is enabled on a bucket, all old and new objects will be replicated
- Replicated objects do not retain metadata

**Giải thích**

SSR và CRR có thể được cấu hình tại S3 bucket level, 1 shared prefix level, hoặc 1 object level sử dụng S3 object tags

S3 lifecycle action hông được sao chép với S3 replication

## Câu 34: Bạn có 1 web app 3-tier chứa 1 web layer sử dụng AngularJS, 1 application layer sử dụng 1 API Gateway và 1 data layer trong RDS database. Web app của bạn cho phép visitors tìm kiếm các moves phổ biến từ quá hứ. Công ty đang tìm cách giảm số api call tới endpoint và cải thiện latency cho API.

Bạn có thể làm gì để cải thiện performance ? 

**Đúng**
- Enable API Gateway Caching

**Sai**
- Use Stage Variables
- Use Mapping Templates
- Use Amazon Kinesis Data Streams to stream incoming data and reduce the burden on Gateway APIs

**Giải thích**



## Câu 35: 1 công ty dịch vụ tài chính muốn đảm bảo dữ liệu của khách hàng luôn giữ được mã hóa trên S3 nhưng muốn AWS quản lý. Giải pháp cho phép full control để create, rotate và remove encryption keys.

Là 1 DVA, Bạn sẽ đưa ra gợi ý gì để giải quyết trường hợp này ? 

**Đúng**
- Server-Side Encryption with Customer Master Keys (CMKs) Stored in AWS Key Management Service (SSE-KMS)

**Sai**
- Server-Side Encryption with Secrets Manager
- Server-Side Encryption with Customer-Provided Keys (SSE-C)
- Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3)

**Giải thích**



## Câu 36: 1 công ty viễn thông cung cấp dịch vụ internet cho thiết bị di động duy trì hơn 100 C4.Large instances trong us-east-1 region. EC2 instance chạy các thuật toán phức tạp. Người quản lý muốn theo dõi CPU utilization của EC2 instance thường xuyên mỗi 10 giây 

Giải pháp nào sau đây là tốt nhất cho trường hợp này ?  

**Đúng**
- Create a high-resolution custom metric and push the data using a script triggered every 10 seconds

**Sai**
- Simply get it from the CloudWatch Metrics
- Enable EC2 detailed monitoring
- Open a support ticket with AWS

**Giải thích**

Muốn track 10s/1 lần thì phải có high-resolution

## Câu 37: 1 công ty muốn 1 giải pháp quản lý tự động và điều phối 1 luồng dữ liệu multi-source high-volume có thể scale được xây dựng sử dụng AWS Services. Giải pháp phải đảm bảo các nguyên tắc doanh nghiệp và biến đối thường xuyên, xử lý lại data trong trường hợp bị lỗi, và yêu cầu khả năng bảo trì tối thiểu 

Dịch vụ AWS nào nên được sử dụng để quản lý và tự động điều phối data flows ? 

**Đúng**
- AWS Step Functions

**Sai**
- Amazon Kinesis Data Streams
- AWS Glue
- AWS Batch

**Giải thích**



## Câu 38: Bạn đã được yêu cầu bởi Team Lead của bạn để cho phép monitoring chi tiết EC2 instances mà team sử dụng. Là 1 DVA làm việc trên AWS CLI, lệnh nào dưới đây sẽ giúp bạn chạy ? 

**Đúng**
- aws ec2 monitor-instances --instance-ids i-1234567890abcdef0

**Sai**
- aws ec2 monitor-instances --instance-id i-1234567890abcdef0
- aws ec2 run-instances --image-id ami-09092360 --monitoring State=enabled
- aws ec2 run-instances --image-id ami-09092360 --monitoring Enabled=true

**Giải thích**



## Câu 39: 1 team dev đã tạo 1 IAM user mới có quyền `s3:putObject` để write 1 S3 bucket. S3 bucket này sử dụng SSE-KMS là encryption mặc định. Đang dùng access key ID và secret access key của IAM user, ứng dụng đã nhận 1 access denied error khi gọi `PutObject` API

Là 1 DVA, bạn sẽ giải quyết vấn đề này thế nào ? 

**Đúng**
- Correct the policy of the IAM user to allow the `kms:GenerateDataKey` action

**Sai**
- Correct the policy of the IAM user to allow the `s3:Encrypt` action
- Correct the bucket policy of the S3 bucket to allow the IAM user to upload encrypted objects
- Correct the ACL of the S3 bucket to allow the IAM user to upload encrypted objects

**Giải thích**

Phải có policy hợp lệ của IAM user cho phép hành động `kms:GenerateDataKey`

## Câu 40: 1 ứng dụng mobile chăm sóc sức khỏe sử dụng các thuật toán ML độc quyền để cung cấp sớm các chuẩn đoán sử dụng các thông số sức khỏe của bệnh nhân. Để bảo vệ dữ liệu nhạy cảm này, team dev muốn chuyển tới 1 hệ thống quản lý user có khả năng scale với tính năng đăng nhập/ đăng ký cũng hỗ trợ MFA

Lựa chọn nào sau đây có thể được sử dụng để triển khai 1 giải pháp với nỗ lực phát triển ít nhất 

**Đúng**
- Use Amazon Cognito for user-management and facilitating the log-in/sign-up process
- Use Amazon Cognito to enable Multi-Factor Authentication (MFA) when users log-in

**Sai**
- Use Amazon SNS to send Multi-Factor Authentication (MFA) code via SMS to mobile app users
- Use Lambda functions and RDS to create a custom solution for user management
- Use Lambda functions and DynamoDB to create a custom solution for user management

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**



## Câu 21:

**Đúng**
-

**Sai**
-
-
-

**Giải thích**
