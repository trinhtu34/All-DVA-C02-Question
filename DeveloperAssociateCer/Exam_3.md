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
