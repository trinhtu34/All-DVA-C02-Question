## Câu 1: Team phát triển ap tại 1 ứng dụng di động game xã hội muốn đơn giản trong việc xử lý đăng nhập cho ứng dụng. Team đang tìm 1 giải pháp có khả năng scale, fully managed cho việc quản lý người dùng để đáp ứng sự phát triển nhanh chóng của ứng dụng theo mong đợi. 

Là 1 Dev Associate, giải pháp nào sau đây bạn sẽ đề xuất để đáp ứng yêu cầu với ít chi phí phát triển nhất ?

**Đúng**
- Use Cognito User pools to facilitate sign up and user management for the mobile app

**Sai**
- Create a custom solution using Lambda and DynamoDB to facilitate sign up and user management for the mobile app
- Use Cognito Identity pools to facilitate sign up and user management for the mobile app
- Create a custom solution using EC2 and DynamoDB to facilitate sign up and user management for the mobile app

**Giải thích**

## Câu 2: Team dev tại 1 công ty thương mại điện tử đã hoàn thành deployment cuối cùng cho ứng dụng của họ với khả năng giảm công suốt bởi vì deployment policy. Ứng dụng bị ảnh hưởng tới hiệu năng do lưu lượng truy cập lớn vào mùa sale.

Lựa chọn nào sau đây đại diện cho giải pháp deployment tốt nhất cho phiên bản ứng dụng sắp tới để đảm bảo nó vẫn duy trì ít nhất là toàn bộ hiệu suất của ứng dụng và giảm thiểu tác động tác động của các deployment bị fail ?

**Đúng**
- Deploy the new application version using 'Immutable' deployment policy

**Sai**
- Deploy the new application version using 'Rolling' deployment policy
- Deploy the new application version using 'Rolling with additional batch' deployment policy
- Deploy the new application version using 'All at once' deployment policy

**Giải thích**

Vì Immuatable sẽ tạo 1 ASG + EC2 hoàn toàn mới và deploy new version trên fleet này. Khi deploy thành công 100% thì switch traffic từ env cũ qua env mới này. Điểm mạnh là full capacity và không ảnh hưởng tới Production. Điểm yếu là chi phí cao vì phải duy trì cả 2 env khi deployment phiên bản mới.

## Câu 3: Là 1 Team Leader, bạn có trách nhiệm lập báo cáo của việc build các code mỗi tuần để báo cáo nội bộ và khách hàng. Báo cáo này nhất quán về việc số lần build trong 1 tuần, và tỉ lệ % giữa build thành công và thất bại, và tổng thời gian tiêu tốn để build bởi thành viên nhóm. Bạn cũng cần lấy Logs cho các  lần buid thất bại và phân tích chúng trên Athena

Lựa chọn nào sẽ giúp đạt được điều này

**Đúng**
- Enable S3 and CloudWatch Logs integration

**Sai**
- Use AWS Lambda integration
- Use AWS CloudTrail and deliver logs to S3
- Use Amazon EventBridge

**Giải thích**

Có thể tích hợp CloudWatch Logs vào AWS CodeBuild để monitoring các thông số như total builds, failed builds, successful builds, và duration of builds. Bạn có thể monitor việc Build của bạn ở 2 cấp độ: Project level, AWS Account level. Bạn có thể export log data từ Log groups ra S3 bucket và sử dụng data này cho việc custom processing và phân tích, hoặc để load vào hệ thống của bạn. 

## Câu 4: Gần đây trong tổ chức của bạn, AWS X-Ray SDK đã được đóng gói vào mỗi Lambda function để ghi lại các outgoing call cho mục đích truy xét ( tracing ). Khi team leader của bạn vào X-Ray service ở AWS Console thì thấy overview của các thông tin đã thu thập được, mà các data này không khả dụng .

Lý do nào giống nhất với vấn đề này ? 

**Đúng**
- Fix the IAM Role

**Sai**
- X-Ray only works with AWS Lambda aliases
- Enable X-Ray sampling
- Change the security group rules

**Giải thích**

X-Ray giúp các Devs phân tích và debug trên Production, ứng dụng phân tán, chẳng hạn như các ứng dụng được xây dựng bằng kiến trúc Microservice. Với X-Ray, bạn có thể hiểu về hiệu suất ứng dụng và các dịch vụ cơ bản để xác định và sửa lỗi gốc rễ vấn đề performance và lỗi. X-Ray cung cấp 1 cái nhìn từ đầu tới cuối của yêu cầu khi traffic đi qua ứng dụng của bạn, và showw 1 map các thành phần cơ bản của ứng dụng.

Và chỉ có các IAM có quyền thì mới có thể view được data của X-Ray

## Câu 5: 1 Team dev tại 1 công ty bán lẻ đa quốc gia muốn hỗ trợ cho các người dùng được xác thực bởi third-party của họ từ nhà cung cấp của tổ chức để tạo và cập nhật các bản ghi cụ thể trong DynamoDB tables trong AWS account của công ty

Là DVA, giải pháp nào dưới đây bạn sẽ đề xuất giải pháp nào cho use-case này

**Đúng**
- Use Cognito Identity pools to enable trusted third-party authenticated users to access DynamoDB

**Sai**
- Create a new IAM group in the company's AWS account for each of the third-party authenticated users from the supplier organizations. The users can then use the IAM group credentials to access DynamoDB
- Create a new IAM user in the company's AWS account for each of the third-party authenticated users from the supplier organizations. The users can then use the IAM user credentials to access DynamoDB
- Use Cognito User pools to enable trusted third-party authenticated users to access DynamoDB

**Giải thích**

Chỉ có Cognito Identity pools mới cung cấp quyền truy cập vào các tài nguyên được chỉ định cho các service ở trong AWS 

## Câu 6: 1 Dev cần tự động triển khai đóng gói phần mềm cho cả 2 EC2 instance và virtual servers vận hành ở on-premeses, là 1 phần của CI/CD mà doanh nghiệp đã áp dụng.

AWS Service nào sẽ giúp anh ấy đáp ứng được công việc này ? 

**Đúng**
- AWS CodeDeploy

**Sai**
- AWS CodeBuild
- AWS CodePipeline
- AWS Elastic Beanstalk

**Giải thích**

Auto deployment thì phải là CodeDeploy, đây là quá trình CD - Continuous Delivery

## Câu 7: 1 Kế toán tại công ty lớn sử dụng EBS volumes cho việc lưu trữ dài hạn cho dữ liệu của ứng dụng trên EC2 instance. Volumes được mã hóa để bảo vệ dữ liệu quan trọng của khách hàng. Là 1 phần của việc quản lý security credentials, PM đã tìm thấy 1 đoạn policy như sau :

```yaml
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Allow for use of this Key",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::111122223333:role/UserRole"
            },
            "Action": [
                "kms:GenerateDataKeyWithoutPlaintext",
                "kms:Decrypt"
            ],
            "Resource": "*"
        },
        {
            "Sid": "Allow for EC2 Use",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::111122223333:role/UserRole"
            },
            "Action": [
                "kms:CreateGrant",
                "kms:ListGrants",
                "kms:RevokeGrant"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                "kms:ViaService": "ec2.us-west-2.amazonaws.com"
            }
        }
    ]
}
```

Lựa chọn nào sau đây là hợp lệ cho policy ở trên ? 

**Đúng**
- The first statement provides a specified IAM principal the ability to generate a data key and decrypt that data key from the CMK when necessary

**Sai**
- The second statement in this policy provides the security group (mentioned in first statement of the policy), the ability to create, list, and revoke grants for Amazon EC2
- The first statement provides the security group the ability to generate a data key and decrypt that data key from the CMK when necessary
- The second statement in the policy mentions that all the resources stated in the first statement can take the specified role which will provide the ability to create, list, and revoke grants for Amazon EC2

**Giải thích**

Đây là KMS key policy, thường dùng cho EBS encryption với EC2. Statement 1 cho phép IAM role cụ thể được phép GenerateDataKeyWithoutPlaintext và Decrypt. Đây là quyền cần cho việc mã hóa và giải mã dữ liệu

## Câu 8: Team dev tại 1 công ty phân tích đang sử dụng SQS queues cho việc tách nhiều thành phần trong kiến trúc ứng dụng. Khi consumers cần thêm thời gian xử lý SQS Message, team dev muốn trì hoãn việc gửi tin nhắn mới vào queue trong vài giây.

Là 1 DVA, giải pháp nào bạn sẽ đề xuất cho team dev

**Đúng**
- Use delay queues to postpone the delivery of new messages to the queue for a few seconds

**Sai**
- Use dead-letter queues to postpone the delivery of new messages to the queue for a few seconds
- Use visibility timeout to postpone the delivery of new messages to the queue for a few seconds
- Use FIFO queues to postpone the delivery of new messages to the queue for a few seconds

**Giải thích**

Delay Queue cho phép hoàn việc delivery message mới tới queue trong vòng vài giây. 

## Câu 9: 1 công ty dược phẩm vận hành database workloads của họ trên IOPS SSD (io1) volumes. Là 1 DVA, lựa chọn nào sau đây mà bạn xác định là không hợp lệ khi cấu hình cho io1 EBS volumes type.

**Đúng**
- 200 GiB size volume with 15000 IOPS

**Sai**
- 200 GiB size volume with 10000 IOPS
- 200 GiB size volume with 2000 IOPS
- 200 GiB size volume with 5000 IOPS

**Giải thích**

Tỉ lệ tối đa của Provisioned IOPS để requested volume size (tính theo GiB) là 50:1. Vì vậy, cho 200GiB volume size, thì max IOPS có thể là 200 * 50 = 10000 IOPS

## Câu 10: 1 ứng dụng hỗ trợ social gaming chuyển quà giữa các người chơi. Khi 1 người dùng đạt được 1 cột mốc nhất định, họ kiếm được 1 mã quà tặng có thể được chuyển đổi hoặc giao dịch với người chơi khác. Team dev muốn đảm bảo việc giao dịch được ghi lại trong database sao cho việc ghi lại trong hồ sơ của cả 2 người chơi đều là thành công hoặc là duy trì trạng thái hiện tại.

Giải pháp nào sau đây đại diện cho lựa chọn tốt nhất để đáp ứng được yêu cầu của use-case này ?

**Đúng**
- Use the DynamoDB transactional read and write APIs on the table items as a single, all-or-nothing operation
- Complete both operations on RDS MySQL in a single transaction block

**Sai**
- Use the Amazon Athena transactional read and write APIs on the table items as a single, all-or-nothing operation
- Complete both operations on Amazon RedShift in a single transaction block
- Perform DynamoDB read and write operations with ConsistentRead parameter set to true

**Giải thích**

Vì DynamoDB hỗ trợ ACID transactions, giúp đảm bảo All-or-nothing, không bị ghi lẹch dữ liệu giữa 2 user. Đây là giải pháp tốt nhất khi dùng DynamoDB

MySQL thì có transaction, đây là giải pháp truyền thống khi dùng SQL 

## Câu 11: 1 công ty thương mại điện tử quản lý 1 ứng dụng Microservice nhận orders từ nhiều partners thông qua 1 customize API cho mỗi Partner được expose thông qua API Gateway. Thứ tự được xử lý bởi 1 Lambda function dùng chung. 

Làm thế nào để công ty có thể thông báo cho mỗi Partners về đơn đặt hàng của họ một cách hiệu quả nhất mà không làm ảnh hưởng tới đơn đặt hàng của partner khác. Do đó giải pháp nên có khả năng đáp ứng Scale cho partners mới cùng với yêu cầu thay đổi code tối thiểu.

**Đúng**
- Set up an SNS topic and subscribe each partner to the SNS topic. Modify the Lambda function to publish messages with specific attributes to the SNS topic and apply the appropriate filter policy to the topic subscriptions

**Sai**
- Set up a separate SNS topic for each partner. Modify the Lambda function to publish messages for each partner to the partner's SNS topic
- Set up a separate Lambda function for each partner. Set up an SNS topic and subscribe each partner to the SNS topic. Modify each partner's Lambda function to publish messages with specific attributes to the SNS topic and apply the appropriate filter policy to the topic subscriptions
- Set up a separate SNS topic for each partner and subscribe each partner to the respective SNS topic. Modify the Lambda function to publish messages with specific attributes to the partner's SNS topic and apply the appropriate filter policy to the topic subscriptions

**Giải thích**

Bởi vì nếu setup SNS hoặc Lambda function mỗi Partners là 1 Subcrible thì nó không ảnh hưởng tới Partners khác và cũng giảm việc tạo SNS và Lambda, chỉ cần 1 cho tất cả Partner

## Câu 12: Bạn là 1 team leader đang cài đặt quyền cho các IAM users khác với quyền tối thiểu. Trên AWS Console, bạn đã tạo 1 Dev Group nơi mà các devs mới sẽ được thêm vào, và trên workstation của bạn, bạn đã cấu hình 1 dev profile. Bạn muốn test user này không muốn terminate instance.

Lựa chọn nào sau đây sẽ giúp bạn thực thi ? 

**Đúng**
- Use the AWS CLI --dry-run option

**Sai**
- Retrieve the policy using the EC2 metadata service and use the IAM policy simulator
- Using the CLI, create a dummy EC2 and delete it using another CLI call
- Use the AWS CLI --test option

**Giải thích**

Với option --dry-run thì sẽ không có EC2 nào bị terminate, đây là cách chuẩn do AWS khuyến nghị để test các Permission 

```
aws ec2 terminate-instances \
  --instance-ids i-1234567890abcdef0 \
  --dry-run \
  --profile dev
```

## Câu 13: Bạn tạo 1 ASG để hoạt động với 1 ALB. ASG được cấu hình với minimum size value là 5, và maximum value là 20, và desired capacity value là 10. 1 trong 10 EC2 instance đã được báo là Unhealthy.

Hành động nào sau đây sẽ giúp thay thế

**Đúng**
- The ASG will terminate the EC2 Instance

**Sai**
- The ASG will format the root EBS drive on the EC2 instance and run the User Data again
- The ASG will keep the instance running and re-start the application
- The ASG will detach the EC2 instance from the group, and leave it running

**Giải thích**

## Câu 14: 1 công ty có 1 Cloud System trên AWS cùng với các thành phần gửi và nhận message sử dụng SQS Queues. Trong khi đang xem xét lại hệ thống bạn thấy nó xử lý nhiều thông tin và muốn 
biết về bất kỳ giới hạn nào của hệ thống.

Lựa chọn nào sau đây đại diện cho số message tối đa mà có thể được lưu trữ trong SQS queue ?

**Đúng**
- no limit

**Sai**
- 10000000
- 10000
- 100000

**Giải thích**

## Câu 15: Trong khi sửa lỗi, 1 dev nhận ra EC2 instance khong thể connect tới internet sử dụng IG

Điều kiện nào sẽ đáp ứng cho hoạt động tới internet có thể được thiết lập

**Đúng**
- The network ACLs associated with the subnet must have rules to allow inbound and outbound traffic
- The route table in the instance’s subnet should have a route to an Internet Gateway

**Sai**
- The instance's subnet is associated with multiple route tables with conflicting configurations
- The instance's subnet is not associated with any route table
- The subnet has been configured to be Public and has no access to the internet

**Giải thích**

Nếu Internet Gateway không kết nối được tới Internet thì chỉ có các lý do như sau : Kiểm tra NACLs có Outbound không, kiểm tra Route Table có phải là public route table không. Kiểm tra Security Group có Outbound ra Internet không 

## Câu 16: Cách gì có thể giúp dev tối ưu performance CPU-bound cho Lambda function và đảm bảo thời gian phản hồi nhanh chóng ?

**Đúng**
- Increase the function's memory

**Sai**
- Increase the function's provisioned concurrency
- Increase the function's timeout
- Increase the function's CPU

**Giải thích**

Chỉ có thể tăng Memory, vì CPU không tăng được, không thể provisioned concurrency được. Tăng timeout chỉ chờ lâu hơn, không giải quyết được gì. 

## Câu 17: 1 công ty đã tạo 1 S3 bucket lưu trữ dữ liệu của khách hàng. Team lead vừa bật tính năng truy cập log cho bucket lưu trữ dữ liệu của khách hàng này. Bucket size đã tăng đáng kể sau khi bắt đầu truy cập log. Khi không có file mới được thêm vào bucket, Team lead đang bối rối trong việc tìm câu trả lời.

Lý do nào sau đây có thể giải thích cho hành động này ? 

**Đúng**
- S3 access logging is pointing to the same bucket and is responsible for the substantial growth of bucket size

**Sai**
- A DDoS attack on your S3 bucket can potentially blow up the size of data in the bucket if the bucket security is compromised during the attack
- Erroneous Bucket policies for batch uploads can sometimes be responsible for the exponential growth of S3 Bucket size
- Object Encryption has been enabled and each object is stored twice as part of this configuration

**Giải thích**

Nếu truy cập log và log tăng nhanh thì khả năng cao là nơi lưu trữ log và bucket cần theo dõi là 1. 

## Câu 18: 1 ứng dụng đang vận hành trên EC2 instance xử lý message từ 1 SQS queue. Tuy nhiêm thỉnh thoảng messages không được xử lý và kết thúc với lỗi. Những message này cần cô lập cho việc xử lý và sửa lỗi xa hơn.

Lựa chọn nào sau đây giúp đạt được điều này ?

**Đúng**
- Implement a Dead-Letter Queue

**Sai**
- Use DeleteMessage
- Reduce the VisibilityTimeout
- Increase the VisibilityTimeout

**Giải thích**

Muốn cô lập Messages lỗi chỉ có thể dùng Dead-Letter Queue

## Câu 19: 1 Dev muốn lưu trữ bảo mật và nhận về nhiều kiểu biến, chẳng hạn như thông tin remote API authentication, API URL, và credentials liên quan trên nhiều môi trường khác nhau của ứng dụng đã deploy trên ECS.

Đâu là cách tiếp cận tốt nhất cần cho việc thay đổi cần thiết trong Code của ứng dụng ?

**Đúng**
- Configure the application to fetch the variables and credentials from AWS Systems Manager Parameter Store by leveraging hierarchical unique paths in Parameter Store for each variable in each environment

**Sai**
- Configure the application to fetch the variables from an encrypted file that is stored with the application by storing the API URL and credentials in unique files for each environment
- Configure the application to fetch the variables from AWS KMS by storing the API URL and credentials as unique keys in KMS for each environment
- Configure the application to fetch the variables from each of the deployed environments by defining the authentication information and API URL in the ECS task definition as unique names during the deployment process

**Giải thích**

Cách tiếp cận tốt nhất cho ứng dụng cần lưu trữ Secrets và được deploy trên AWS là dùng các service lưu trữ Secret như : Secret Manager, SSM Parameter Store.

## Câu 20: 1 công ty đang vận hành ứng dụng cao cấp của họ trên 1 nhóm EC2 instance. Sau khi lạc mất 1 cặp Private key của SSH Key Pairs, họ đã quyết định sử dụng lại SSH Key Pairs của instance trên Region khác. 

Là 1 DVA, lời khuyên nào sau đây sẽ giúp giải quyết vấn đề này ?

**Đúng**
- Generate a public SSH key from a private SSH key. Then, import the key into each of your AWS Regions

**Sai**
- Encrypt the private SSH key and store it in the S3 bucket to be accessed from any AWS Region
- Store the public and private SSH key pair in AWS Trusted Advisor and access it across AWS Regions
- It is not possible to reuse SSH key pairs across AWS Regions

**Giải thích**

Các bước để sử dụng lại SSH key từ 1 private ssh key:
1. Tạo 1 public ssh key (.pub) file từ private ssh key (.pem) file
2. Set region bạn muốn import vào
3. Import public ssh key từ region mới 

## Câu 21: 1 Dev trong công ty của bạn vừa được thăng chức làm Team Leader và sẽ phụ trách việc deployment code trên EC2 instance thông qua CodeCommit và CodeDeploy. Trên yêu cầu mới, việc xử lý deployment nên có thể thay đổi quyền truy cập cho việc deploy file cũng như xác minh việc deploy thành công.

Hành động nào giúp anh Dev mới ? 

**Đúng**
- Define an appspec.yml file in the root directory

**Sai**
- Define an appspec.yml file in the codebuild/ directory
- Define a buildspec.yml file in the codebuild/ directory
- Define a buildspec.yml file in the root directory

**Giải thích**

Vì mình đang cần thay đổi quyền cho hành động deploy, vì CodeDeploy dùng file appspec.yml và phải đặt file này ở root directory

## Câu 22: 1 Dev đang làm việc với EC2 Windows instance đã tải Kinesis Agent cho Windows để stream JSON-formatted log file tới S3 thông qua KDF. Dev muốn hiểu về khả năng xử lý các loại đầu ra của Kinesis Data Firehose (KDF)

Loại xử lý nào không hỗ trợ bởi KDF ? 

**Đúng**
- Amazon ElastiCache with Amazon S3 as backup

**Sai**
- Amazon Redshift with Amazon S3
- Amazon Elasticsearch Service (Amazon ES) with optionally backing up data to Amazon S3
- Amazon Simple Storage Service (Amazon S3) as a direct Firehose destination

**Giải thích**

ElastiCache không được hỗ trợ là destination cho KDF

## Câu 23: là 1 Senior Dev, bạn được giao nhiệm vụ cùng với team dev của minh tạo vài API được hỗ trợ bởi API Gateway. Các dev đang làm việc trên API trong dev env, nhưng họ thấy thay đổi được thực hiện với API mà không phản ánh vào API khi API được call.

Là Dev, giải pháp nào sau đây bạn nên khuyên cho vấn đề này ?

**Đúng**
- Redeploy the API to an existing stage or to a new stage

**Sai**
- Developers need IAM permissions on API execution component of API Gateway
- Use Stage Variables for development state of API
- Enable Lambda authorizer to access API

**Giải thích**

Nếu thay đổi code trong API Gateway mà không deploy lại vào Stage thì code mới chưa apply được.

## Câu 24: 1 ứng dụng serverless xây dựng trên AWS xử lý đơn đặt hàng của khách hàng 24/7 sử dụng Lambda function và giao tiếp với HTTP API nội bộ của vendor cho việc xử lý thanh toán. Team dev muốn thông báo cho team hỗ trợ ở mức near real-time sử dụng 1 SNS topic, nhưng chỉ khi API nội bộ bị lỗi khoảng 5% tổng transaction được xử lý trong 1 tiếng.

là 1 DVA, lựa chọn nào bạn sẽ đề xuất cho giải pháp hiệu quả nhất ? 

**Đúng**
- Configure and push high-resolution custom metrics to CloudWatch that record the failures of the external payment processing API calls. Create a CloudWatch alarm that sends a notification via the existing SNS topic when the error rate exceeds the specified rate

**Sai**
- Configure CloudWatch metrics with detailed monitoring for the external payment processing API calls. Create a CloudWatch alarm that sends a notification via the existing SNS topic when the error rate exceeds the specified rate
- Log the results of payment processing API calls to Amazon CloudWatch. Leverage Amazon CloudWatch Metric Filter to look at the CloudWatch logs. Set up the Lambda function to check the output from CloudWatch Metric Filter on a schedule and send notification via the existing SNS topic when the error rate exceeds the specified rate
- Log the results of payment processing API calls to Amazon CloudWatch. Leverage Amazon CloudWatch Logs Insights to query the CloudWatch logs. Set up the Lambda function to check the output from CloudWatch Logs Insights on a schedule and send notification via the existing SNS topic when the error rate exceeds the specified rate

**Giải thích**

Vì API nội bộ không có metric sẵn trên CloudWatch. Và cần high-resolution để có thể cảnh báo gần tới mức real time. 

## Câu 25: Hãy xem xét 1 ứng dụng cho phép người dùng lưu chữ hình ảnh điện thoại di động của họ trên Cloud và hỗ trợ 10000 users. Ứng dụng nên sử dụng 1 AWS API Gateway để tận dụng AWS Lambda function cho việc xử lý hình ảnh trong khi lưu trữ chi tiết hình trong DynamoDB. Ứng dụng nên cho phép người dùng tạo 1 account, upload hình ảnh, và nhận về hình ảnh đã upload cùng với hình ảnh có kích thước trong khoảng từ 500KB tới 5MB.

Bạn sẽ thiết kế ứng dụng như thế nào cùng với ít chi phí hoạt động nhất ?

**Đúng**
- Leverage Cognito user pools to manage user accounts and set up an Amazon Cognito user pool authorizer in API Gateway to control access to the API. Set up a Lambda function to store the images in Amazon S3 and save the image object's S3 key as part of the photo details in a DynamoDB table. Have the Lambda function retrieve previously uploaded images by querying DynamoDB for the S3 key

**Sai**
- Leverage Cognito user pools to manage user accounts and set up an Amazon Cognito user pool authorizer in API Gateway to control access to the API. Set up a Lambda function to store the images as well as the image metadata in a DynamoDB table. Have the Lambda function retrieve previously uploaded images from DynamoDB
- Use Cognito identity pools to create an IAM user for each user of the application during the sign-up process. Leverage IAM authentication in API Gateway to control access to the API. Set up a Lambda function to store the images in Amazon S3 and save the image object's S3 key as part of the photo details in a DynamoDB table. Have the Lambda function retrieve previously uploaded images by querying DynamoDB for the S3 key
- Use Cognito identity pools to manage user accounts and set up an Amazon Cognito identity pool authorizer in API Gateway to control access to the API. Set up a Lambda function to store the images in Amazon S3 and save the image object's S3 key as part of the photo details in a DynamoDB table. Have the Lambda function retrieve previously uploaded images by querying DynamoDB for the S3 key

**Giải thích**

Dùng Cognito user pools để quản lý tài khoản người dùng.
Dùng Cognito user pool authorizer trong API Gateway để control access.
Dùng Lambda để lưu hình trong S3 và lưu S3 key của hình và chi tiết của hình trong DynamoDB.
Dùng Lambda để truy xuất hình đã tải bằng query DynamoDB theo S3 key.

## Câu 26: 1 Dev đang tìm cách thiết lập access control cho 1 API kết nối tới Lmabda function ở phía sau. Cơ chế nào không thể dùng cho việc xác thực với API Gateway ?

**Đúng**
- AWS Security Token Service (STS)

**Sai**
- Lambda Authorizer
- Cognito User Pools
- Standard AWS IAM roles and policies

**Giải thích**

AWS STS là 1 web service cho phép bạn request tạm, có credentials hạn chế cho IAM user hoặc cho users mà bạn đã xác thực. Tuy nhiên, nó không hỗ trợ API Gateway

## Câu 27: 1 Dev team đang xây 1 game nơi mà các player có thể mua vật phẩm bằng coin ảo. Mỗi coin ảo đã mua bởi 1 user, cả 2 players table cũng như item table trong DynamoDB cần update ngay lập tức sử dụng 1 hành động của DynamoDB là All-Or-Nothing 

Là 1 DVA, bạn sẽ triển khai tính năng này như thế này ? 

**Đúng**
- Use `TransactWriteItems` API of DynamoDB Transactions

**Sai**
- Capture the transactions in the players table using DynamoDB streams and then sync with the items table
- Capture the transactions in the items table using DynamoDB streams and then sync with the players table
- Use `BatchWriteItem` API to update multiple tables simultaneously

**Giải thích**

Nếu dùng `TransactWriteItems` hỗ trợ nhiều item, nhiều table khác nhau, ACID transaction. Nếu có 1 operation fail sẽ rollback toàn bộ. Trong trường hợp này trừ coin của player đồng thời ghi nhận item đã mua.

## Câu 28: 1 ứng dụng CRM được host trên EC2 instances với database tier sử dụng DynamoDB. Khách hàng đã nâng cao quyền riêng tư và bảo mật đề cập tới việc gửi và nhận dữ liệu qua public internet.

Là 1 DVA, bạn đề xuất giải pháp nào sau đây để tối ưu giao tiếp giữa EC2 instance và DynamoDB sử dụng Public Internet.

**Đúng**
- Configure VPC endpoints for DynamoDB that will provide required internal access without using public internet

**Sai**
- Create a NAT Gateway to provide the necessary communication channel between EC2 instances and DynamoDB
- The firm can use a virtual private network (VPN) to route all DynamoDB network traffic through their own corporate network infrastructure
- Create an Internet Gateway to provide the necessary communication channel between EC2 instances and DynamoDB

**Giải thích**

Các cách có thể sử dụng để kết nối tới DynamoDB:
- VPC endpoint
- NAT Gateway
- Internet Gateway

## Câu 29: 1 công ty cần 1 Version Control System cho việc vòng đời triển khai của họ nhanh với thay đổi tăng dần, version control, và hỗ trợ Git tools hiện tại.

AWS Service nào sẽ đáp ứng được yêu cầu này ? 

**Đúng**
- AWS CodeCommit

**Sai**
- Amazon Versioned S3 Bucket
- AWS CodePipeline
- AWS CodeBuild

**Giải thích**

## Câu 30: 1 công ty muốn tự động quy trình xử lý đơn hàng và hàng tồn kho. Bắt đầu từ tạo đơn đặt hàng tới cập nhật kho tới giao hàng, toàn bộ quy trình được track, quản lý và update tự động.

Bạn sẽ khuyên thế nào cho giải pháp tối ưu nhất cho yêu cầu này ? 

**Đúng**
- Use AWS Step Functions to coordinate and manage the components of order management and inventory tracking workflow

**Sai**
- Use Amazon Simple Queue Service (Amazon SQS) queue to pass information from order management to inventory tracking workflow
- Use Amazon SNS to develop event-driven applications that can share information
- Configure Amazon EventBridge to track the flow of work from order management to inventory tracking systems

**Giải thích**

## Câu 31: 1 Dev đang xây 1 ứng dụng serverless trên AWS và muốn thiết lập 1 workflow phát triển nhanh chóng. Workflow phải cho phép dev deploy với những thay đổi tăng dần cho việc test mà không deploy toàn bộ ứng dụng cho mỗi commit. Các Dev muốn streamline cái quá trình trong khi giảm thiểu thời gian deploy. 

Dev làm gì để đáp ứng yêu cầu này ? 

**Đúng**
- Use the sam sync command from the AWS Serverless Application Model (AWS SAM) to deploy incremental changes

**Sai**
- Use the sam deploy command from the AWS Serverless Application Model (AWS SAM) to deploy incremental changes
- Use the cdk deploy command from the AWS Cloud Development Kit (AWS CDK) to deploy incremental changes to AWS for testing
- Use the cdk diff command from the AWS Cloud Development Kit (AWS CDK) to deploy incremental changes to AWS for testing

**Giải thích**

`sam sync` được thiết kế để cho phép nhanh chóng lặp lại đồng bộ hóa các thay đổi ở local với việc deploy serverless application trên AWS. Điều này cho phép các dev nhanh chóng test các thay đổi lặp lại không làm deploy toàn bộ stack.

## Câu 32: 1 trường đại học đã tạo 1 cổng sinh viên có thể truy cập thông qua điện thoại thông minh cả ứng dụng và web. Ứng dụng trên điện thoại available với cả 2 Android và IOS và ứng dụng web hoạt động trên các trình duyệt phổ biến. Học sinh sẽ có thể tạo nhóm học online và tạo forum questions. Tất cả thay đổi thực hiện thông qua điện thoại thông minh nên được available kể cả khi Offline và nên đồng bộ với các thiết bị khác. 

AWS Service nào sau đây sẽ đáp ứng được yêu cầu này ?

**Đúng**
- AWS AppSync

**Sai**
- Cognito User Pools
- Cognito Identity Pools
- BeanStalk

**Giải thích**

AWS AppSync là dịch vụ GraphQL được quản lý, được thiết kế cho việc xây dựng ứng dụng có khả năng collab trong real time. Với tính năng giống như truy cập data khi offline, giải quyết xung đột, và đồng bộ thiết bị.

## Câu 33: 1 công ty dược phẩm sử dụng EC2 instances cho việc hosting ứng dụng của họ và CloudFront cho CDN. 1 bài nghiên cứu với những phát hiện quan trọng cần được chia sẻ với team trên toàn thế giới.

Giải pháp nào sau đây giải quyết được yêu cầu này mà không làm tổn hại đến tính bảo mật của nội dung ? 

**Đúng**
- Use CloudFront signed URL feature to control access to the file

**Sai**
- Configure AWS Web Application Firewall (WAF) to monitor and control the HTTP and HTTPS requests that are forwarded to CloudFront
- Using CloudFront's Field-Level Encryption to help protect sensitive data
- Use CloudFront signed cookies feature to control access to the file

**Giải thích**

Chỉ có Signed URL mới có thể truy cập được file, không cần login, expired, IP restriction, chia sẻ 1 file cụ thể. Đây là best practice AWS cho bảo mật nội dung cá nhân phân tán toàn cầu thông qua CloudFront

## Câu 34: 1 doanh nghiệp đã mua 1 Reserved instance là m4.xlarge nhưng đã sử dụng 3 m4.xlarge instance cùng lúc trong 1 giờ.

Là 1 Dev, bạn hãy giải thích cách tính tiền của instance ? 

**Đúng**
- One instance is charged at one hour of Reserved Instance usage and the other two instances are charged at two hours of On-Demand usage

**Sai**
- All instances are charged at one hour of Reserved Instance usage
- All instances are charged at one hour of On-Demand Instance usage
- One instance is charged at one hour of On-Demand usage and the other two instances are charged at two hours of Reserved Instance usage

**Giải thích**

## Câu 35: 1 doanh nghiệp có môi trường test của họ được xây dựng trên EC2 được cấu hình trên General purpose SSD volume. Tại `gp2` volume size nào sẽ đạt được max IOPS ? 

**Đúng**
- 5.3 TiB

**Sai**
- 2.7 TiB
- 16 TiB
- 10.6 TiB

**Giải thích**

`gp2` tối đa có 16000 IOPS. 3 IOPS / 1 GB. => size = 16000 / 3 = 5333GB = 5.3TB

## Câu 36: Team dev tại 1 công ty bán lẻ đang chuẩn bị cho đợt sale lễ giáng sinh và muốn đảm bảo backend của ứng dụng hoạt động thông qua Lambda function không bị latency bottlenecks khi traffic tăng cao bất ngờ.

Là 1 DVA, giải pháp nào dưới đây bạn sẽ dùng để giải quyết trường hợp này ?

**Đúng**
- Configure Application Auto Scaling to manage Lambda provisioned concurrency on a schedule

**Sai**
- Add an Application Load Balancer in front of the Lambda functions
- No need to make any special provisions as Lambda is automatically scalable because of its serverless nature
- Configure Application Auto Scaling to manage Lambda reserved concurrency on a schedule

**Giải thích**

Để tránh latency khi speak traffic thì chỉ cần nhớ dùng : Application Auto Scaling cho việc Provisioned concurrency 

## Câu 37: 1 công ty truyền thông đang dùng EC2 instance cho việc vận hành ứng dụng kinh doanh quan trọng của họ. Team IT của họ đang tìm cách tiết kiệm 1 phần capacity trong savings plan của họ cho các instances quan trọng.

là 1 DVA, Bạn sẽ chọn reserved instance type  ?  

**Đúng**
- Zonal Reserved Instances

**Sai**
- Neither Regional Reserved Instances nor Zonal Reserved Instances
- Both Regional Reserved Instances and Zonal Reserved Instances
- Regional Reserved Instances

**Giải thích**

Vì Reserve capacity tại 1 AZ cụ thể thì AWS sẽ cam kết có chỗ chạy EC2 không như Regional sẽ không có cam kết đảm bảo.

## Câu 38: 1 startup đang thử nghiệm DynamoDB trong test env. Team dev đã phát hiện một số write openrations ghi đè các item đang tồn tại ở 1 primary key. Điều này gây ra việc tăng data rất lớn, dẫn tới sai lệch dữ liệu.

Lựa chọn DynamoDB write nào bạn sẽ chọn để tránh loại overwriting này ? 

**Đúng**
- Conditional writes

**Sai**
- Atomic Counters
- Use Scan operation
- Batch writes

**Giải thích**

## Câu 39: 1 công ty sử dụng CodeDeploy để deploy ứng dụng từ GitHub lên EC2 instance vận hành trên Linu. Quá trình deploy sử dụng 1 file gọi là appspec.yml cho việc deploy hooks cụ thể. 1 final lifecycle event nên được chỉ định để verify deloyment thành công.

Hook events nào nên được sử dụng để verify các deployment thành công ?

**Đúng**
- ValidateService

**Sai**
- AllowTraffic
- ApplicationStart
- AfterInstall

**Giải thích**

Lifecyle của CodeDeploy :
1. BeforeInstall
2. AfterInstall
3. ApplicationStart
4. ValidateService

Vì vậy ValidateService là hook cuối cùng nên dùng để Health check, smoke test, verify app chạy đúng 

## Câu 40: 1 công ty dùng SQS để Tiết kiệm chi phí khi gửi email đến các khách hàng đã đăng ký. Thỉnh thoảng, SES service ném ra lỗi: `Throttling – Maximum sending rate exceeded`. là DVA, bạn sẽ đưa ra giải pháp nào để sửa lỗi này ? 

**Đúng**
- Use Exponential Backoff technique to introduce delay in time before attempting to execute the operation again

**Sai**
- Implement retry mechanism for all 4xx errors to avoid throttling error
- Raise a service request with Amazon to increase the throttling limit for the SES API
- Configure Timeout mechanism for each request made to the SES service

**Giải thích**

Exponential Backoff là : fail lần 1 -> đợi n giây, lần 2 -> m giây,...

Vì đây là lỗi thỉnh thoảng nên chưa nên gửi phiếu tăng quota

## Câu 41: 1 dev đang cấu hình EC2 ASG để dynamic scaling. Metric nào phía dưới hông phải là 1 phần của Target Tracking Scaling Policy ? 

**Đúng**
- ApproximateNumberOfMessagesVisible

**Sai**
- ASGAverageCPUUtilization
- ALBRequestCountPerTarget
- ASGAverageNetworkOut

**Giải thích**

ApproximateNumberOfMessagesVisible là thông số của SQS

## Câu 42: Ngoài `Resources` section, Resource ssections nào sau đây là bắt buộc dùng cho SAM template

**Đúng**
- Transform

**Sai**
- Parameters
- Globals
- Mappings

**Giải thích**

## Câu 43: Team dev tại 1 công ty chăm sóc sức khỏe đã deploy EC2 instance trong AWS Account A. Các instance này cần truy cập dữ liệu của bệnh nhận trên nhiều S3 Bucket trong AWS Account B.

Là 1 DVA, giải pháp nào sau đây nên dùng cho trường hợp này ?

**Đúng**
- Correct answer
Create an IAM role with S3 access in Account B and set Account A as a trusted entity. Create another role (instance profile) in Account A and attach it to the EC2 instances in Account A and add an inline policy to this role to assume the role from Account B

**Sai**
- Create an IAM role (instance profile) in Account A and set Account B as a trusted entity. Attach this role to the EC2 instances in Account A and add an inline policy to this role to access S3 data from Account B
- Copy the underlying AMI for the EC2 instances from Account A into Account B. Launch EC2 instances in Account B using this AMI and then access the PII data on Amazon S3 in Account B
- Add a bucket policy to all the Amazon S3 buckets in Account B to allow access from EC2 instances in Account A

**Giải thích**

Thêm 1 IAM role bằng S3 account trong Account Bvaf set Account A là 1 trusted entity. Tạo role khác là instance profile và gắn nó vào EC2 instance trong Account A và thêm 1 inline policy cho role này để assume role từ Account B

## Câu 44: 1 junior Dev đã được giao nhiệm vụ để cấu hình truy cập 1 EC2 instance hosting 1 web app. Dev đã cấu hình 1 SG để cho phép incoming HTTP trafic từ 0.0.0.0/0 và giữ lại mọi default rules 1 custom NACL đã kết nối với subnet của instance đã cấu hình để cho phép incoming HTTP traffic từ 0.0.0.0/0 và giữ lại mọi default outbound rules.

Giải pháp nào bạn sẽ đề xuất nếu EC2 instance cần để accept và phản hồi các yêu cầu từ internet ?

**Đúng**
- An outbound rule must be added to the Network ACL (NACL) to allow the response to be sent to the client on the ephemeral port range

**Sai**
- The configuration is complete on the EC2 instance for accepting and responding to requests
- Outbound rules need to be configured both on the security group and on the NACL for sending responses to the Internet Gateway
- An outbound rule on the security group has to be configured, to allow the response to be sent to the client on the HTTP port

**Giải thích**

Phải allow outbound rule cho NACL vì đây là stateless, không cần cấu hình cho SG vì SG là stateful vì vậy nên SG đã có sẵn outbound khi cấu hình Inbound

## Câu 45: Trong khi định nghĩa 1 business workflow là State Machine trên AWS Step Functions, 1 dev đã cấu hình vài States.

Bạn sẽ xác định state nào sau đây đại diện cho 1 single unit of work thực hiện bởi 1 State Machine

**Đúng**

```
"HelloWorld": {
  "Type": "Task",
  "Resource": "arn:aws:lambda:us-east-1:123456789012:function:HelloFunction",
  "Next": "AfterHelloWorldState",
  "Comment": "Run the HelloWorld Lambda function"
}
```

**Sai**
```
"No-op": {
  "Type": "Task",
  "Result": {
    "x-datum": 0.381018,
    "y-datum": 622.2269926397355
  },
  "ResultPath": "$.coords",
  "Next": "End"
}
```
```
"wait_until" : {
  "Type": "Wait",
  "Timestamp": "2016-03-14T01:59:00Z",
  "Next": "NextState"
}
```
```
"FailState": {
  "Type": "Fail",
  "Cause": "Invalid response.",
  "Error": "ErrorA"
}
```

**Giải thích**

## Câu 46: 1 data analytics đang xử lý dữ liệu IoT real-time thông qua Kinesis Producer Library (KPL) và gửi dữ liệu tới ứng dụng được điều khiển bởi KDS. Ứng dụng đã dừng xử lý dữ liệu bởi vì `ProvisionedThroughputExceeded `

Hành động nào sau đây sẽ giúp giải quyết vấn đề này ? 

**Đúng**
- Configure the data producer to retry with an exponential backoff
- Increase the number of shards within your data streams to provide enough capacity

**Sai**
- Use Amazon SQS instead of Kinesis Data Streams
- Use Kinesis enhanced fan-out for Kinesis Data Streams
- Use Amazon Kinesis Agent instead of Kinesis Producer Library (KPL) for sending data to Kinesis Data Streams

**Giải thích**

Nếu lỗi `ProvisionedThroughputExceeded` là KDS không đủ throughput để xử lý lưu lượng hiện tại. Lỗi này xảy ra khi :
1. Ghi / Đọc quá nhiều records / bytes vào shard
2. Vượt quá giới hạn: 1MB/s write, 1000 records/s, 2 MB/s/shard

## Câu 47: Bạn đã launch vài AWS Lambda function được viết bằng Java. 1 yêu cầu mới là cần truyền hơn 1MB data tới functions và mã hóa / giải mã trong quá trình thực thi.

Phương thức nào sau đây có khả năng giải quyết trường hợp này ? 

**Đúng**
- Use Envelope Encryption and reference the data as file within the code

**Sai**
- Use KMS direct encryption and store as file
- Use Envelope Encryption and store as environment variable
- Use KMS Encryption and store as environment variable

**Giải thích**

Envelope Encryption = Best practice của AWS: dùng KMS encrypt data key, Data key encrypt lớn phù hợp với Data > 1MB, Runtime encryption/description

Envelope Encryption là kỹ thuật mã hóa Data key. Dùng khi dữ liệu cần mã hóa 1 lượng lớn dữ liệu. Data key mã hóa Data chính, KMS mã hóa Data key.

## Câu 48: 1 DVA, bạn được thuê để làm việc cùng với team dev tại 1 công ty để tạo 1 REST API sử dụng serverless architecture.

Giải pháp nào sau đây bạn sẽ chọn để di chuyển công ty thành kiến trúc serverless.

**Đúng**
- API Gateway exposing Lambda Functionality

**Sai**
- Fargate with Lambda at the front
- Public-facing Application Load Balancer with ECS on Amazon EC2
- Route 53 with EC2 as backend

**Giải thích**

## Câu 49: là 1 dev, bạn đã viết 1 document bằng YAML thể hiện kiến trúc serverless application. Dòng đầu tiên của tài liệu chứa `Transform: 'AWS::Serverless-2016-10-31`.

`Transform` section trong tài liệu đại diện cho cái gì ?

**Đúng**
- Presence of Transform section indicates it is a Serverless Application Model (SAM) template

**Sai**
- It represents an intrinsic function
- It represents a Lambda function definition
- Presence of Transform section indicates it is a CloudFormation Parameter

**Giải thích**

## Câu 50: 1 dev đang xác định những người đăng ký để có thể tạo signed URLs cho CloudFront distributions của họ.

Statement nào sau đây nên được dev cân nhắc trong khi xác định người đăng ký ? 

**Đúng**
- When you create a signer, the public key is with CloudFront and private key is used to sign a portion of URL
- When you use the root user to manage CloudFront key pairs, you can only have up to two active CloudFront key pairs per AWS account

**Sai**
- Both the signers (trusted key groups and CloudFront key pairs) can be managed using the CloudFront APIs
- CloudFront key pairs can be created with any account that has administrative permissions and full access to CloudFront resources
- You can also use AWS Identity and Access Management (IAM) permissions policies to restrict what the root user can do with CloudFront key pairs

**Giải thích**

Signer là đối tượng dược phép tạo Signed URL/ Signed Cookie để người dùng truy cập nội dung CloudFront bị giới hạn, có 2 cách:
1. CloudFront key pairs ( cũ ): gắn trực tiếp với root user, có Public key + private key, không nên dùng cho hệ thống mới. 
2. Trusted key groups ( mới): dùng Public key, gắn với distribution, Private key dùng ở app/backend để ký URL.

## Câu 51: bạn là 1 dev đang làm việc trên 1 Web app được viết bởi Java và muốn sử dụng Elastic Beanstalk cho việc deployment bởi vì nó xử lý deployment, capacity provisioning, load balancing, auto-scaling, application health monitoring. Trong quá khứ, bạn đã kết nối với provisioned instances thông qua SSH để dùng lệnh cấu hình. Bây giờ, bạn muốn cơ chế cấu hình tự động applies settings cho bạn. Lựa chọn nào giúp bạn làm điều này ?

**Đúng**
- Include config files in .ebextensions/ at the root of your source code

**Sai**
- Use an AWS Lambda hook
- Deploy a CloudFormation wrapper
- Use SSM parameter store as an input to your Elastic Beanstalk Configurations

**Giải thích**

`.ebextensions` là thư mục đặc biệt của Elastic Beanstalk chứa các file `.config`(YAML/JSON), được tự động chạy khi environment khởi tạo hoặc update.

`.ebextensions` có thể cài package, chỉnh file config, set .env, chỵ command/script, config apache/nginx/jvm options/os level setting. Sẽ thay thế toàn bộ việc SSH thủ công.

## Câu 52: Team lead của bạn đã giao cho bạn học AWS CloudFormation để tạo 1 bộ sưu tập liên quan AWS Resource và provision chúng theo thứ tự. Bạn quyết định cung cấp các loại giá trị tham số hợp lệ được AWS chỉ định. Khi chỉ định tham số đau là Parameter type không hợp lệ ?  

**Đúng**
- DependentParameter

**Sai**
- CommaDelimitedList
- AWS::EC2::KeyPair::KeyName
- String

**Giải thích**

Các Parameter types cho phép CloudFormation để validate input sớm hơn trong quá trình tạo stack
``` 
String – A literal string
Number – An integer or float
List<Number> – An array of integers or floats
CommaDelimitedList – An array of literal strings that are separated by commas
AWS::EC2::KeyPair::KeyName – An Amazon EC2 key pair name
AWS::EC2::SecurityGroup::Id – A security group ID
AWS::EC2::Subnet::Id – A subnet ID
AWS::EC2::VPC::Id – A VPC ID
List<AWS::EC2::VPC::Id> – An array of VPC IDs
List<AWS::EC2::SecurityGroup::Id> – An array of security group IDs
List<AWS::EC2::Subnet::Id> – An array of subnet IDs
```

## Câu 53: Là 1 Senior Architect, bạn chịu trách nhiệm cho việc phát triển, hỗ trợ, bảo trì và triển hai tất cả database application được viết sử dụng công nghệ NoSQL. 1 dự án mới yêu cầu thông lượng 10 lần read/s, mỗi lần có kích thước 6KB.

Có bao nhiêu capacity unit mà bạn sẽ cần hi cấu hình DynamoDB Table của bạn ? 

**Đúng**
- 20

**Sai**
- 30
- 10
- 60

**Giải thích**

1 RCU = 1 strongly consistent read/s cho item <= 4KB

Nếu item > 4 KB thì làm tròn lên bội số của 4KB 

item size = 4KB => 6KB làm tròn lên 8KB => 1 request cần 2 RCU => 10 request/s = 10 x 2 = 20 RCU

## Câu 54: 1 Doanh nghiệp host website trên EC2 instances và sử dụng Auto Scaling để điều chỉnh resource theo traffic tăng đột biến. Tuy nhiên, users toàn cầu báo Loading chậm bởi vì static content được host trên EC2 instance tốn quá nhiều thời gian để load, ngay cả không phải ở trong thời điểm busy.

Hành động gì giúp cải thiện latency của website ? 

**Đúng**
- Transfer the application’s static content hosted on EC2 instances to Amazon S3
- Set up an Amazon CloudFront distribution to cache the static content with Amazon S3 configured as the origin

**Sai**
- Upgrade the CPU and RAM available to the EC2 instances
- Migrate the application to AWS Lambda
- Double the Auto Scaling group’s desired capacity

**Giải thích**

Chuyển qua Host Static content trên S3 và dùng CloudFront distribution để làm CDN giúp giảm Latency bằng Edge Location.

## Câu 55: 1 công ty đang sử dụng AWS VPN dựa vào Border Gateway Protocol (BGP) để kết nối từ on-premises data center tới EC2 instances trong tài khoản công ty của họ. Team dev có thể truy cập EC2 instance từ Subnet A nhưng không thể truy cập EC2 instance trong Subnet B trong cùng VPC.

Logs nào sau đây có thể được sử dụng để xác định bất kỳ traffic nào truy cập subnet B ? 

**Đúng**
- VPC Flow Logs

**Sai**
- BGP logs
- Subnet logs
- VPN logs

**Giải thích**

## Câu 56: 1 Team dev đang làm việc trên AWS Lambda functions để tru cập DynamoDB. Lambda function phải thực hiện cập nhật/thêm mới, Nó phải trả về 1 item và cập nhật một số thuộc tính hoặc tạo item nếu nó hông tồn tại 

Lựa chọn nào sau đây thể hiện giải pháp tối thiểu IAM permission có thể được dùng cho Lmabda function để đạt được tính năng này ? 

**Đúng**
- dynamodb:UpdateItem, dynamodb:GetItem

**Sai**
- dynamodb:AddItem, dynamodb:GetItem
- dynamodb:GetRecords, dynamodb:PutItem, dynamodb:UpdateTable
- dynamodb:UpdateItem, dynamodb:GetItem, dynamodb:PutItem

**Giải thích**

Permission `UpdateItem` tự động làm được upsert (update/insert), item không tồn tại thì tạo mới, không cần gọi PutItem.

Ở đây cần minimum permission cho update, insert vì vậy dùng UpdateItem.

## Câu 57: 1 ASG có maximum capacity là 3, hiện tại capacity là 2, và 1 Scaling Policy thêm 3 instances. Khi thực thi scaling policy, outcome ghi nhận là gì ? 

**Đúng**
- Amazon EC2 Auto Scaling adds only 1 instance to the group

**Sai**
- Amazon EC2 Auto Scaling adds 3 instances to the group
- Amazon EC2 Auto Scaling adds 3 instances to the group and scales down 2 of those instances eventually
- Amazon EC2 Auto Scaling does not add any instances to the group, but suggests changing the scaling policy to add one instance

**Giải thích**

ASG không bao giờ vượt quá MaxCapacity, hiện tại 2 instance, max 3 instance, policy + 3 instance => max - current = 3 - 2 = 1

## Câu 58: 1 công ty muốn chia sẻ thông tin với bên thứ 3 thông qua 1 HTTP API endpoint được quản lý bởi partner thứ 3. Công ty có API key cần thiết để truy cập endpoint và tích hợ API Key với code ứng dụng của công ty phải không ảnh hưởng tới Performance của ứng dụng.

Đâu là cách tiếp cận bảo mật nhất ?

**Đúng**
- Keep the API credentials in AWS Secrets Manager and use the credentials to make the API call by fetching the API credentials at runtime by using the AWS SDK

**Sai**
- Keep the API credentials in an encrypted table in MySQL RDS and use the credentials to make the API call by fetching the API credentials from RDS at runtime by using the AWS SDK
- Keep the API credentials in a local code variable and use the local code variable at runtime to make the API call
- Keep the API credentials in an encrypted file in S3 and use the credentials to make the API call by fetching the API credentials from S3 at runtime by using the AWS SDK

**Giải thích**

Dùng AWS Secrets Manager để lưu credentials

## Câu 59: Sau khi review code, 1 dev đã được giao làm cái public S3 bucket trở nên private, và cho phép truy cập objects cùng với 1 time-bound constraint.

Lựa chọn nào sau đây sẽ giải quyết trường hợp này ? 

**Đúng**
- Share pre-signed URLs with resources that need access

**Sai**
- Use Bucket policy to block the unintended access
- Use Routing policies to re-route unintended access
- It is not possible to implement time constraints on Amazon S3 Bucket access
 
**Giải thích**

Truy cập trong 1 khoảng thời gian cố định thì dùng Pre-signed URL.

## Câu 60: Công ty của bạn đón nhận Cloud-Native Microservice Architectures. Ứng dụng mới phải được dockerized và được lưu trong 1 Registry Service được cung cấp bởi AWS. Kiến trúc nên hỗ trợ dynamic port mapping và hỗ trợ multiple tasks từ 1 service đơn lẻ trong cùng container instance. Tất cả service  nên chạy trong cùng EC2 instance. 

Lựa chọn nào sau đây cung cấp giải pháp tốt nhất cho trường hợp này ?

**Đúng**
- Application Load Balancer + ECS

**Sai**
- Classic Load Balancer + ECS
- Classic Load Balancer + Beanstalk
- Application Load Balancer + Beanstalk

**Giải thích**

Kiến trúc sử dụng là Microservice -> Cần Dockerized => ECS

Muốn lưu trong 1 Registry Service được cung cấp bởi AWS => ECR

Dynamic Port Mapping => ECS: ECS có thể gán container port cố định, nhưng host port động

Multi tasks từ 1 Service, chạy tất cả trên cùng 1 EC2 instance 

## Câu 61: Là 1 DVA, bạn đang làm việc trên 1 CloudFormation template sẽ tạo resources cho 1 infra Cloud của công ty. Template của bạn được bao gồm 3 stacks: Stack-A, Stacks-B, và Stack-C. Stack-A sẽ cấp phát 1 VPC, a SG, và subnet cho public web application sẽ được tham chiếu đến Stack-B và Stack-C.

Sau khi chạy các Stacks bạn quyết định delete chúng, Thứ tự nào bạn sẽ làm nó

**Đúng**
- Stack B, then Stack C, then Stack A
 
**Sai**
- Stack A, then Stack B, then Stack C
- Stack C then Stack A then Stack B
- Stack A, Stack C then Stack B

**Giải thích**

## Câu 62: 1 phòng chuẩn đoán lưu trữ dữ liệu trong DynamoDB. Phòng chuẩn đoán muốn backup dữ liệu của 1 DynamoDB table cụ thể vào S3, vì vậy nó có thể download S3 backup về nội bộ để cho các công việc cần dùng.

Lựa chọn nào sau đây không khả thi ?

**Đúng**
- Use the DynamoDB on-demand backup capability to write to Amazon S3 and download locally

**Sai**
- Use Hive with Amazon EMR to export your data to an S3 bucket and download locally
- Use AWS Glue to copy your table to Amazon S3 and download locally
- Use AWS Data Pipeline to export your table to an S3 bucket in the account of your choice and download locally

**Giải thích**

DynamoDB On-Demand Backup không ghi ra S3, chỉ  backup được lưu nội bộ trong DynamoDB. Chỉ dùng để Restore lại DynamoDB table, Point-in-time recovery. Không thể xem file backup, copy ra S3, download về Local.

## Câu 63: 1 Dev muốn đóng gói code và Dependencies cho ứng dụng dùng Lambda function khi Container images được host trên ECR. Lựa chọn nào sau đây là hợp lệ cho yêu cầu ?

**Đúng**
- AWS Lambda service does not support Lambda functions that use multi-architecture container images
- To deploy a container image to Lambda, the container image must implement the Lambda Runtime API

**Sai**
- Lambda supports both Windows and Linux-based container images
- You can test the containers locally using the Lambda Runtime API
- You can deploy Lambda function as a container image, with a maximum size of 15 GB

**Giải thích**

Để deploy 1 container images lên Lambda, Container images phải triển khai Lambda Runtime. Lambda Runtime là 1 AWS Open-source runtime interface client để triển khai API. Bạn có thể thêm 1 runtime interface client vào base images ưu tiên của bnaj để làm nó tương thích với Lambda.  

AWS Lambda service không hỗ trợ lambda functions sử  dụng multi-architecture container images. 

## Câu 64: Team tech tại 1 Investment bank sử dụng DynamoDB để thuận tiện cho high-frequecy trading nơi nhiều giao dịch có thể try và update 1 item tại cùng 1 thời điểm

Hành động nào sau đây sẽ đảm bảo chỉ có giá trị được update cuối của bất kỳ item sẽ được sử dụng trong ứng dụng

**Đúng**
- Use ConsistentRead = true while doing GetItem operation for any item

**Sai**
- Use ConsistentRead = true while doing UpdateItem operation for any item
- Use ConsistentRead = true while doing PutItem operation for any item
- Use ConsistentRead = false while doing PutItem operation for any item

**Giải thích**

`GetItem` là read operation khi đặt `ConsistentRead = true` thì DynamoDB đảm bảo giá trị đọc được là bản **Cập nhật mới nhất**, không bị đọc dữ liệu cũ.

## Câu 65: 1 Dev truy cập vào AWS Management Console đã terminate 1 instance trong `us-east-1a` AZ. EBS volume vẫn còn và bây giờ available để gắn cho các instances khác. Đồng nghiệp của bạn launch 1 Linux EC2 instance trong `us-east-1e` AZ và đang cố gắng gắn EBS volume. Đồng nghiệp của bạn không thể thực hiện được và đang cần sự giúp đỡ của bạn. Lời giải thích nào sau đây mà bạn sẽ cung cấp cho họ ? 

**Đúng**
- EBS volumes are AZ locked

**Sai**
- The required IAM permissions are missing
- EBS volumes are region locked
- The EBS volume is encrypted

**Giải thích**

1  EBS volume chỉ dùng cho 1 AZ thôi