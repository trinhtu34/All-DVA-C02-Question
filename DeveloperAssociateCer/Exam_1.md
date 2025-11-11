# Tất cả câu hỏi trong 6 đề thi AWS Certified Developer Associate - DVA C02

---

## Câu 16: Xử lý lỗi ProvisionedThroughputExceededException trong Kinesis Data Streams

**Tình huống:**
Một công ty phân tích dữ liệu IoT sử dụng Kinesis Data Streams (KDS). Team Dev đã nhận thấy dữ liệu đi vào IoT tăng lên đáng kể. PutRecords API call thỉnh thoảng bị fail và log cho thấy response như sau:

```json
HTTP/1.1 200 OK
x-amzn-RequestId: <RequestId>
Content-Type: application/x-amz-json-1.1
Content-Length: <PayloadSizeBytes>
Date: <Date>
{
    "FailedRecordCount": 2,
    "Records": [
        {
            "SequenceNumber": "49543463076548007577105092703039560359975228518395012686",
            "ShardId": "shardId-000000000000"
        },
        {
            "ErrorCode": "ProvisionedThroughputExceededException",
            "ErrorMessage": "Rate exceeded for shard shardId-000000000001 in stream exampleStreamName under account 111111111111."
        },
        {
            "ErrorCode": "InternalFailure",
            "ErrorMessage": "Internal service failure."
        }
    ]
}
```

**Câu hỏi:**
Là một Developer Associate, lựa chọn nào bạn sẽ khuyên để giải quyết trường hợp này?

**Các lựa chọn:**

- A. Giảm số lượng KCL consumers
- B. Giảm tần suất hoặc size của requests
- C. Sử dụng cơ chế error retry và exponential backoff
- D. Gộp các shards để giảm số lượng các shards trong stream
- E. Tăng tần suất hoặc size của requests

**✅ Đáp án đúng:** B và C

**Giải thích:**

✅ **Đúng:**
- **B. Giảm tần suất hoặc size của requests:** Khi gặp ProvisionedThroughputExceededException, nghĩa là bạn đang vượt quá throughput limit của shard. Giảm tần suất hoặc kích thước request giúp giảm tải.
- **C. Sử dụng error retry và exponential backoff:** Đây là best practice khi xử lý throttling errors. Exponential backoff giúp tự động retry với khoảng thời gian tăng dần, tránh làm quá tải hệ thống.

❌ **Sai:**
- **A. Giảm số lượng KCL consumers:** KCL consumers đọc dữ liệu từ stream, không liên quan đến việc ghi dữ liệu (PutRecords).
- **D. Gộp các shards:** Gộp shards sẽ GIẢM throughput capacity, làm vấn đề tệ hơn.
- **E. Tăng tần suất hoặc size của requests:** Điều này sẽ làm vấn đề throttling trầm trọng hơn.

---

## Câu 17: Mô tả nào là best practice về cách KMS Encription hoạt động ?

**Đúng**
- KMS lưu trữ CMK (Customer Master Key), và nhận dữ liệu từ Clients, cái mà nó được mã hóa và gửi lại.

**Sai**
- KMS gửi CMK tới client, CMK thực hiện mã hóa và sau đó delete CMK
- KMS nhận CMK từ client mỗi Encrypt call, và mã hóa cũng với dữ liệu đó
- KMS tạo 1 CMK mới mỗi Encrypt call và mã hóa dữ liệu với nó

**Giải thích**
- Ứng dụng gửi data -> KMS quá HTTPS -> KMS dùng CMK tương ứng để mã hóa data ở bên trong KMS -> KMS trả lại ciphertext (Dữ liệu đã mã hóa). Tương tự khi thực hiện gọi Decrypt API thì dữ liệu mã hóa được gửi tới KMS và KMS giải mã nội bộ -> trả về plaintext 

## Câu 18: Một công ty thương mại điện tử toàn cầu muốn thực hiện geographic load testing API xử lý đơn đặt hàng của họ. Công ty phải deploy resource vào multiple AWS Regions để hỗ trợ load testing API. Công ty đáp ứng yêu cầu này thế nào mà không làm thêm Code cho ứng dụng.

**Đúng**
- Setup AWS CloudFormation template để định nghĩa load test resources. Tận dụng AWS CLI lệnh create-stack-set để tạo 1 stack set trong desired Regions 

**Sai**
- Set up an AWS Cloud Development Kit (CDK) ToolKit that defines the load test resources. Leverage the CDK CLI to create a stack from the template in each Region
- Set up an AWS Organizations template that defines the load test resources across the organization. Leverage the AWS CLI create-stack-set command to create a stack set in the desired Regions
- Set up an AWS CloudFormation template that defines the load test resources. Develop region-specific Lambda functions to create a stack from the AWS CloudFormation template in each Region when the respective function is invoked

**Giải thích**
- CloudFormation cho phép define resource bằng template (YAML/JSON)
- StackSets cho phép triển khai cùng một template tới nhiều accounts hoặc regions trong 1 lệnh CLI hoặc giao diện console 
- CloudFormation đảm bảo viêc tạo các stack trong từng region tự động.

## Câu 19: Một dev đang test SQS queue trong dev env. Queue cùng với toàn bộ nội dung của nó phải xóa sau khi thử nghiệm. SQS API nào nên được sử dụng cho yêu cầu này ?

**Đúng**
- DeleteQueue

**Sai**
- PurgeQueue 
- RemovePermission 
- RemoveQueue is no exist 

**Giải thích**
- DeleteQueue xóa hoàn toàn Queue và mọi thông tin bên trong

## Câu 20: Một dev team muốn xây dựng 1 app sử dụng serverless architecture. Team lên kế hoạch sử dụng toàn bộ AWS Lambda functions để đạt được mục đích. Các Devs của team làm việc trên các ngôn ngữ lập trình khác nhau giống như Python .NET và JS. Team muốn mô hình Cloud Infra sử dụng bất kỳ ngôn ngữ lập trình nào. AWS Service/Tool nào mà team Dev nên sử dụng cho trường hợp này ?

**Đúng**
- AWS Cloud Development Kit (CDK)

**Sai**
- AWS CloudFormation
- AWS Serverless Application Model (SAM)
- AWS CodeDeploy

**Giải thích**
- CDK là 1 phần mềm open-source để định nghĩa resource trên Cloud sử dụng ở rất nhiều ngôn ngữ lập trình 

## Câu 21: Tổ chức toàn cầu của bạn có 1 IT Infra được triển khai sử dụng CloudFormation trên AWS. Một nhân viên trong us-east-1 region đã tạo 1 stack `Application1` và để nó export output có tên là `ELBDNSName`. Một nhân viên khác đã tạo 1 stack cho 1 application khác tên là `Appliction2` trong us-east-2 region và cũng export ra 1 output có tên là `ELBDNSName`. Nhân viên đầu tiên muốn deploy CloudFormation stack `Application1` trong us-east-2, nhưng đã nhận 1 lỗi. Lý do của lỗi là gì ? 

**Đúng**
- Exported Output Values in CloudFormation must have unique names within a single Region

**Sai**
- Output Values in CloudFormation must have unique names across all Regions
- Output Values in CloudFormation must have unique names within a single Region
- Exported Output Values in CloudFormation must have unique names across all Regions

**Giải thích**
- Vì Export names phải là duy nhất trong cùng 1 region của cùng 1 tài khoản AWS

## Câu 22: Một multi-national company có nhiều đơn vị kinh doanh trên mỗi đơn vị thì có AWS account của họ. Team Dev tại công ty muốn debug và trace data trên nhiều accounts và visualize nó trên 1 centralized account. Giải pháp nào bạn sẽ đề xuất cho trường hợp này ? 

**Đúng**
- X-Ray

**Sai**
- EventBridge
- CloudTrail
- VPC Flow Logs

**Giải thích**
- X-Ray giúp debug các request xuyên qua nhiều AWS Service như API GW, Lambda, EC2, ECS, ...
- Hỗ trợ cross-acoount tracing, để gửi trace data từ nhiều account khác nhau. Tập trung lưu và hiển thị tất cả trace trong 1 central account
- X-Ray console trong tài khoản trung tâm có thể hiển thị end-toend flow của request - cực hữu ích khi debug hệ thống phân tán

## Câu 23: Bạn là 1 Dev trong 1 công ty sản xuất có nhiều server on-site. Công ty quyết định di chuyển lên Cloud sử dụng công nghệ serverless. Bạn quyết định dùng AWS SAM và làm việc với 1 SAM template file để thể hiện serverless architecture của bạn . Lựa chọn nào là không hợp lệ cho 1 loại serverless resource type ? 

**Đúng**
- AWS::Serverless::UserPool

**Sai**
- AWS::Serverless::Function
- AWS::Serverless::SimpleTable
- AWS::Serverless::Api

**Giải thích**
- SAM supports the following resource types:
    - AWS::Serverless::Api
    - AWS::Serverless::Application
    - AWS::Serverless::Function
    - AWS::Serverless::HttpApi
    - AWS::Serverless::LayerVersion
    - AWS::Serverless::SimpleTable
    - AWS::Serverless::StateMachine

## Câu 24: Tech Lead của team bạn đã review 1 CloudFormation YAML được viết bởi 1 nhân viên mới và đã chỉ ra 1 section không hợp lệ được add vào template. Đoạn nào là section không hợp lệ trong CloudFormation template

**Đúng**
- 'Dependencies' section of the template

**Sai**
- 'Resources' section of the template
- 'Conditions' section of the template
- 'Parameters' section of the template

**Giải thích**
- Một số section hợp lệ : AWSTemplateFormatVersion , Description , Metadata , Parameters , Mappings, Conditions, Resources, Outputs

## Câu 25: Một startup vừa tạo 1 tài khoản AWS mới đang test các EC2 instance khác nhau. Họ đã sử dụng Burstable performance instance như T2.micro trong 35 giây và đã dừng instace.  

**Đúng**
- 0 seconds

**Sai**
- 35 seconds
- 60 seconds
- 30 seconds

**Giải thích**

## Câu 26: Là một phần của Dev team, 1 ông DEV đang tạo pilicies và gắn chúng vào IAM identities. Sau khi tạo các identity-based policies cần thiết, anh ấy đang tạo Resouce-based policies. IAM service hỗ trợ duy nhất resource-based policy nào ?

**Đúng**
- Trust policy

**Sai**
- Permissions boundary
- AWS Organizations Service Control Policies (SCP)
- Access control list (ACL)

**Giải thích**

## Câu 27: Là 1 AWS DVA, bạn đã nhận 1 tài liệu được viết bằng YAML, nó đại diện cho 1 kiến trúc serverless application. Dòng đầu tiên có ràng buộc `Transform: 'AWS::Serverless-2016-10-31'.` . Section `Transform` trong tài liệu này đại diện cho cái gì ? 

**Đúng**
- Presence of `Transform` section indicates it is a Serverless Application Model (SAM) template

**Sai**
- Presence of Transform section indicates it is a CloudFormation Parameter
- It represents a Lambda function definition
- It represents an intrinsic function

**Giải thích**
- Transform được dùng để chỉ định template này sẽ được xử lý (transform) bởi 1 macro cụ thể trước khi CloudFormation triển khai nó.
- khi có `Transform: 'AWS::Serverless-2016-10-31'` thì có nghĩa là :
    - Template này là 1 AWS SAM template
    - AWS CloudFormation sẽ sử dụng `AWS::Serverless transform macro` để expand các resouce kiểu `AWS::Serverless::*` 

## Câu 28: Security credential nào sau đây chỉ có thể được tạo bởi AWS Account root ? 

**Đúng**
- CloudFront Key Pairs

**Sai**
- IAM User Access Keys
- EC2 Instance Key Pairs
- IAM User passwords

**Giải thích**

## Câu 29: Bạn là 1 dev đang làm trong 1 ứng dụng xử lý đơn đặt hàng ở mức large scale. Sau khi phát triển tính năng, bạn commit code vào CodeCommit và bắt đầu xây dựng project với CodeBuild trước khi nó được deploy lên Server. thời gian Build quá lâu và chỉ ra lỗi từ dependencies từ bên thứ 3. Bạn muốn ngăn chặn việc Buld chạy lâu trong tương lai với lý do tương tự. Lựa chọn nào đại diện cho best solution để giải quyết trường hợp này ?

**Đúng**
- Enable CodeBuild timeouts

**Sai**
- Use Amazon CloudWatch
- Use VPC Flow Logs
- Use AWS Lambda

**Giải thích**
- CodeBuild có thể set project settings -> timeout theo phut . Nếu dependency bị treo hoặc quá chậm, build sẽ dừng sớm để tiết kiệm thời gian và chi phí.

## Câu 30: Bạn đã tạo 1 Java App sử dụng RDS để lưu trữ dữ liệu chính và ElastiCache để lưu user session. Ứng dụng cần được deploy sử dụng Elastic Beanstalk và mỗi deploy mới nên cho phép app servers sử dụng lại RDS Database. Mặt khác, dữ liệu user sesion được lưu trữ trong ElastiCache có thể được tạo lại cho mỗi deployment. Cấu hình nào sẽ cho phép bạn đạt được điều này ? 

**Đúng**
- ElastiCache defined in `.ebextensions/`
- RDS database defined externally and referenced through environment variables

**Sai**
- ElastiCache database defined externally and referenced through environment variables
- RDS database defined in `.ebextensions/`
- ElastiCache bundled with the application source code

**Giải thích**
- Tạo 1 RDS riêng ngoài Elastic Beanstalk và cấu hình thông tin RDS trong env của EB. Cách này đảm bảo RDS không bị xóa khi redeploy hoặc terminate environment, vì nó không được tạo bởi EB.
- `.ebextension/` là thư mục trong ứng dụng EB chứa các file cấu hình YAML/JSON. Bạn có thể dùng nó để tạo các sub-resources như ElastiCache, SNS topic, S3 bucket,... Mỗi khi environment được tạo hoặc redeploy.

## Câu 31: Bạn chọn Elastic Beanstalk để upload application code của bạn và cho phép nó xử lý chi tiết chẳng hạn như provisioning resources và monitoring. Khi tạo file cấu hình cho Elastic Beanstalk, bạn nên tuân thủ quy ước đặt tên nào ? 

**Đúng**
- `.ebextensions/<mysettings>.config`

**Sai**
- `.ebextensions_<mysettings>.config`
- `.config_<mysettings>.ebextensions`
- `.config/<mysettings>.ebextensions`

**Giải thích**
- Tất cả file phải đặt trong thư mục `.ebextensions/` ở thư mục root của source code. File phải có đuôi `.config`

## Câu 32: Một công ty an ninh mạng muốn chạy ứng dụng của họ trên single-tenant hardware để đáp ứng hướng dẫn bảo mật. Cách nào sau đây là tiết kiệm chi phí nhất và cô lập EC2 instances trên 1 single-tenant  

**Đúng**
- Dedicated Instances : chạy trên physical hardware riêng, nhưng AWS vẫn quản lý server.

**Sai**
- Dedicated Hosts : kiểm soát physical hardware nhưng rất đắt
- Spot Instances : rẻ nhưng có thể bị terminate bất ngờ
- On-Demand Instances : không phải là single-tenant

**Giải thích**

## Câu 33: Một công ty muốn cải thiện performance cho API service public cung cấp khả năng read access không cần xác thực vào thông tin thống kê đươc cập nhật hàng ngày, thông qua API Gateway và Lambda. Công ty có thể thực hiện biện pháp nào ? 

**Đúng**
- Enable API caching in API Gateway : đây là built-in cache, tốt cho unauthorized read heavy 

**Sai**
- Configure API Gateway to use Elasticache for Memcached : ElastiCache không phải là built-in cache
- Configure API Gateway to use Gateway VPC Endpoint : VPC Endpoint giúp private communication giữa VPC và API Gateway, không tăng tốc API cho client public.
- Set up usage plans and API keys in API Gateway : Chỉ kiểm soát rate limit và access, không tăng hiệu suất hoặc cache response.

**Giải thích**

## Câu 34: Một công ty thương mại điện tử đã phát triển 1 API được host trên ECS. Nhiều traffic spikes trên application gây ra quá trình xử lý đặt hàng quá lâu. Ứng dụng xử lý đơn đặt hàng ử dụng SQS queue. thông số `ApproximateNumberOfMessagesVisible` tăng đột biến ở thông lượng cao trong suốt cả ngày, kích hoạt CLoudWatch Alarm. Các ECS metrics khác cho API containers vẫn ở trong hạn mức.  

**Đúng**
- Use backlog per instance metric with target tracking scaling policy

**Sai**
- Use ECS service scheduler
- Use Docker swarm
- Use ECS step scaling policy

**Giải thích**
- Backlog per instance metric : tính toán số message mỗi instance ECS đang xử lý. Giúp tự động scale ECS tasks dựa trên lượng task thực tế, không chỉ CPU/Memory. 
- Target tracking scaling policy: ECS sẽ tự động tăng/giảm số task để giữ backlog per instance gần target. Điều này giúp giảm Latency xử lý orders khi traffic tăng đột biến. Giữ chi phí thấp khi traffic thấp (Không scale quá mức).

## Câu 35: Một công ty đang tao 1 ứng dụng game sẽ được deploy trên các thiết bị mobile. Ứng dụng sẽ gửi data tới 1 RESTful API dựa vào Lambda function. Ứng dụng sẽ assign mỗi API request 1 identifier duy nhất. Volume của API requests từ ứng dụng có thể ngẫu nhiên ở bất kỳ lúc nào trong ngày. Trong quá trình request throttling ( điều tiết request ), ứng dụng có thể cần retry request. API phải có thể đáp ứng duplicate requests mà không làm không nhất quán hoặc mất dữ liệu. Lựa chọn nào mà bạn sẽ khuyên để xử lý các yêu cầu này.

**Đúng**
- Persist the unique identifier for each request in a DynamoDB table. Change the Lambda function to check the table for the identifier before processing the request

**Sai**
- Persist the unique identifier for each request in a DynamoDB table. Change the Lambda function to send a client error response when the function receives a duplicate request
- Persist the unique identifier for each request in an ElastiCache for Memcached cache. Change the Lambda function to check the cache for the identifier before processing the request
- Persist the unique identifier for each request in an RDS MySQL table. Change the Lambda function to check the table for the identifier before processing the request

**Giải thích**
- Lambda nhận request -> Check DynamoDB xem request ID đã tồn tại chưa -> chưa thì xử lý và lưu request ID vào table. Nếu request ID đã tồn tại thì trả kết quả luôn -> tránh duplicate
- Serverless + idempotency -> lưu request ID trong DynamoDB, kiểm tra trước khi Xử lý `pattern được khuyến nghị cho idempotency API design`

## Câu 36: Một team dev tại 1 công ty truyền thông số sử dụng AWS Lambda functions cho serverless stack trên AWS. Vì 1 deployment mới, Team Leader chỉ muốn gửi 1 phần traffic nhất định tới phiên bản Lambda mới. Trong trường hợp deployment lỗi, giải pháp cũng nên hỗ trợ khả nawg rollback về phiên bản trước của Lambda, cùng với downtime tối thiểu cho ứng dụng. Lựa chọn nào mà bạn sẽ gợi ý để giải quyết vấn đề này ?

**Đúng**
- Set up the application to use an alias that points to the current version. Deploy the new version of the code and configure the alias to send 10% of the users to this new version. If the deployment goes wrong, reset the alias to point all traffic to the current version

**Sai**
- Set up the application to directly deploy the new Lambda version. If the deployment goes wrong, reset the application back to the current version using the version number in the ARN
- Set up the application to use an alias that points to the current version. Deploy the new version of the code and configure alias to send all users to this new version. If the deployment goes wrong, reset the alias to point to the current version
- Set up the application to have multiple alias of the Lambda function. Deploy the new version of the code. Configure a new alias that points to the current alias of the Lambda function for handling 10% of the traffic. If the deployment goes wrong, reset the new alias to point all traffic to the most recent working alias of the Lambda function

**Giải thích**

## Câu 37: Team dev vừa cấu hình và gắn IAM policy cần thiết để truy cập Billing and Cost Management tới tất cả user trong phòng Finance của họ. Nhưng, các user không thể thấy AWS Billing and Cost Management service ở trong AWS Console. Lý do cho vấn đề này là gì ?

**Đúng**
- You need to activate IAM user access to the Billing and Cost Management console for all the users who need access

**Sai**
- Only root user has access to AWS Billing and Cost Management console
- The users might have another policy that restricts them from accessing the Billing information
- IAM user should be created under AWS Billing and Cost Management and not under AWS account to have access to Billing console

**Giải thích**
- Phải Active IAM user, cho phép IAM user xem Billing bằng root account thì IAM users mới xem được

## Câu 38: 1 Dev đang cấu hình 1 bucket policy đang denies upload object tới tất cả request không có header `x-amz-server-side-encryption` khi request server-side encryption bằng SSE-KMS cho 1 S3 bucket `examplebucket`. Policy nào là vừa đúng cho yêu cầu này ?

**Đúng**
```json
{
   "Version":"2012-10-17",
   "Id":"PutObjectPolicy",
   "Statement":[{
         "Sid":"DenyUnEncryptedObjectUploads",
         "Effect":"Deny",
         "Principal":"*",
         "Action":"s3:PutObject",
         "Resource":"arn:aws:s3:::examplebucket/*",
         "Condition":{
            "StringNotEquals":{
               "s3:x-amz-server-side-encryption":"aws:kms"
            }
         }
      }
   ]
}
```

**Sai**
```json
         "Condition":{
            "StringNotEquals":{
               "s3:x-amz-server-side-encryption":"aws:AES256"
            }
         }
```
```json
         "Condition":{
            "StringNotEquals":{
               "s3:x-amz-server-side-encryption":"false"
            }
         }
```
```json
         "Condition":{
            "StringEquals":{
               "s3:x-amz-server-side-encryption":"aws:kms"
            }
         }

```

**Giải thích**

## Câu 39: 1 Dev có 1 ứng dụng lưu dữ liệu trong S3 bucket. Ứng dụng dùng 1 HTTP API để lưu và lấy đối tượng. Khi thực hiện hành động PutObject để thêm đối tượng vào S3 thì Dev phải mã hóa đối tượng ở điểm cuối bằng cách sử dụng server-side encryption bằng SSE-S3. Giải pháp nào sẽ đảm bảo bất kỳ request upload nào mà không có mã hóa bắt buộc sẽ không được xử lý ?

**Đúng**
- Invoke the PutObject API operation and set the x-amz-server-side-encryption header as AES256. Use an S3 bucket policy to deny permission to upload an object unless the request has this header

**Sai**
- Invoke the PutObject API operation and set the x-amz-server-side-encryption header as aws:kms. Use an S3 bucket policy to deny permission to upload an object unless the request has this header
- Set the encryption key for SSE-S3 in the HTTP header of every request. Use an S3 bucket policy to deny permission to upload an object unless the request has this header
- Invoke the PutObject API operation and set the x-amz-server-side-encryption header as sse:s3. Use an S3 bucket policy to deny permission to upload an object unless the request has this header. Vì `sse:s3` không đúng cú pháp

**Giải thích**
- `x-amz-server-side-encryption: AES256` mới đúng cú pháp

## Câu 40: 1 Dev tại 1 công ty đang làm việc trên CloudFormation template để setup resource. Resource sẽ được định nghĩa sử dụng code và được provision dựa vào 1 điều kiện được định nghĩa trong phần `Condition`. Section nào của CloudFormation template không thể liên kết với `Condition` ? 

**Đúng**
- Parameters : chỉ dùng để nhận giá trị đầu vào khi tạo stack, không thể conditionally tồn tại hoặc biến đổi 

**Sai**
- Resources: có thể tạo resource dựa trên Condition, ex: Condition: MyCondition
- Outputs : Có thể conditionally xuất giá trị, ex: Condition: MyCondition
- Conditions : đây là nơi định nghĩa các điều kiện, không thể gán condition cho chính nó

**Giải thích**

## Câu 41: Bạn là 1 dev cho 1 web app được viết bằng .NET, sử dụng AWS SDK. Bạn cần implement 1 cơ chế Authen và trả về 1 JWT. AWS Service nào là dùng ok

**Đúng**
- Cognito User Pools

**Sai**
- Cognito Sync
- Cognito Identity Pools
- API Gateway

**Giải thích**

## Câu 42: 1 doanh nghiệp kinh doanh thương mại điện tử, có ứng dụng của họ xây dựng trên 1 nhóm EC2 instance, tách ra nhiều Region và AZs. Team Tech có đề xuất sử dụng ELB cho thiết kế kiến trúc tốt hơn. Thành phần gì của ELB là tốt cho lựa chọn này ?

**Đúng**
- Build a highly available system
- Separate public traffic from private traffic

**Sai**
- Improve vertical scalability of the system
- Deploy EC2 instances across multiple AWS Regions
- The Load Balancer communicates with the underlying EC2 instances using their public IPs

**Giải thích**

## Câu 43: CodeCommit là 1 dịch vụ version control, nó host prrivate Git repo trên AWS Cloud. Loại credentials nào sau đây không được hỗ trợ bởi IAM cho CodeCommit.

**Đúng**
- IAM username and password

**Sai**
- SSH Key
- AWS Access Keys
- Git credentials

**Giải thích**

## Câu 44: 1 dev team leader chiu trách nhiệm quản lý quyền truy cập cho các IAM. Tại lúc bắt đầu chu kỳ, cô ấy gán quyền cho người dùng để duy trì động lực cho họ thử những thứ mới. Bây giờ cô ấy muốn đảm bảo rằng team chỉ có minimum permission được yêu cầu để hoàn thành công việc. Điều nào sau đây giúp cô ấy xác định IAM roles không sử dụng và xóa chúng mà không làm gián đoạn bất kỳ dịch vụ nào

**Đúng**
- IAM Access Analyzer

**Sai**
- AWS Trusted Advisor
- Amazon Inspector
- AWS Security Hub

**Giải thích**

## Câu 45: 1 công ty game muốn lưu thông tin về tất cả game mà công ty đã release. Mỗi game có 1 name, version number, category ( chẳng hạn như : sports, puzzles, strategy, etc...). Thông tin trò chơi cũng có thể bao gồm các thuộc tính thêm về nền tảng được hỗ trợ và công nghệ chỉ định. Thêm nữa, thông tin là không nhất quán giữa các game. Bạn cần xây dựng 1 giải pháp để giải quyết các trường hợp sau : 
    - Đối với Name và version number, lấy tất cả chi tiết về game có cùng name và version number 
    - Đối với Name, lấy tất cả chi tiết về tất cả game có cùng Name
    - Về Category, lấy tất cả chi tiết về tất cả game cùng loại
Bạn sẽ đưa ra giải pháp nào là hiệu quả nhất 

**Đúng**
- Set up an Amazon DynamoDB table with a primary key that consists of the name as the partition key and the version number as the sort key. Create a global secondary index that has the category as the partition key and the name as the sort key.

**Sai**
- Set up an Amazon RDS MySQL instance having a games table that contains columns for name, version number, and category. Configure the name column as the primary key
- Set up an Amazon DynamoDB table with a primary key that consists of the category as the partition key and the version number as the sort key. Create a global secondary index that has the name as the partition key
- Permanently store the name, version number, and category information about the games in an Amazon Elasticache for Memcached instance

**Giải thích**

## Câu 46: Bạn đã deploy 1 ứng dụng Java lên 1 EC2 instance, nó dùng X-Ray SDK. Khi test từ máy tính c

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
