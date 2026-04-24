## Câu 14: A dev team has been using S3 service as an object store. With S3 turning strongly consistent, the team wants to understand the impact of this change on its data storage practices. As a dev, can you identify the key characteristics of the strongly consistent data model followed by S3 

**Đúng**
- A process deletes an existing object and immediately tries to read it. Amazon S3 will not return any data as the object has been deleted
- If you delete a bucket and immediately list all buckets, the deleted bucket might still appear in the list

**Sai**
- A process replaces an existing object and immediately tries to read it. Amazon S3 might return the old data
- A process deletes an existing object and immediately tries to read it. Amazon S3 can return data as the object deletion has not yet propagated completely
- A process deletes an existing object and immediately lists keys within its bucket. The object could still be visible for few more minutes till the change propagates

**Giải thích**

Khi xóa 1 đối tượng trong S3 bucket mà nó lại được read thì ngay lập tức sẽ không trả về dữ liệu

Nếu delete 1 bucket và có 1 lượt list bucket thì vẫn có thể bucket đó sẽ được list ra.

## Câu 15: As an DVA, you are writing a CloudFormation template in YAML. The template consists of an EC2 instance creation and one RDS resource. Once your resource are created you would like to output the connection endpoint for the RDS database. which intrinsic function returns the value needed ? 

**Đúng**
- !GetAtt

**Sai**
- !Sub
- !Ref
- !FindInMap

**Giải thích**



## Câu 16: as a dev, you are working on a mobile application that utilizes SQS for sending messages to downstream systems for further processing. One of the requirements is that the messages should be stored in the queue for a period of 12 days. How will you configure this requirement ? 

**Đúng**
- Change the queue message retention setting

**Sai**
- The maximum retention period of SQS messages is 7 days, therefore retention period of 12 days is not possible
- Use a FIFO SQS queue
- Enable Long Polling for the SQS queue

**Giải thích**

Giới hạn tối đa period là 14 ngày cho messages trong SQS

## Câu 17: Your company has a load balancer in a VPC configured to be internet facing. The public DNS name assigned to the load balancer is `myDns-1234567890.us-east-1.elb.amazonaws.com`. When your client application first load they capture the load balancer DNS name and then resolve the IP address for the load balancer so that they can directly reference the underlying IP. It is observed that the client applications work well but unexpectedly stop working after a while. What is the reason for this ? 

**Đúng**
- The load balancer is highly available and its public IP may change. The DNS name is constant

**Sai**
- You need to disable multi-AZ deployments
- Your security groups are not stable
- You need to enable stickiness

**Giải thích**

LB là internet facing, có DNS. Client làm công việc Resolve DNS -> lấy IP -> Dùng IP trực tiếp. Sau một thời gian hoạt động ok thì bị lỗi.

AWS có thể thêm/remove nodes, thay đổi IP backend, scale theo traffic, kết quả dẫn tới IP address không cố định nhưng DNS name luôn cố định

Sai lầm phía Client khi Resolve DNS -> cache IP -> dùng IP trực tiếp 

Vậy nên khi AWS thay đổi IP mà Client vẫn dùng IP cũ -> fail 

Cách làm đúng: Luôn gọi bằng DNS name hoặc Respect DNS TTL -> Resolve lại theo TTL 

## Câu 18: A company has AWS Lambda functions where each is invoked by other AWS services such as KDF, API Gateway, S3, EventBridge. What these lambda functions have in common is that they process heavy workloads such as big data analysis, large file, processing, and statistical computations. 

**Đúng**
- Increase the RAM assigned to your Lambda function

**Sai**
- Change the instance type for your Lambda function
- Change your Lambda function runtime to use Golang
- Increase the Lambda function timeout

**Giải thích**



## Câu 19: Your team-mate has configured an S3 event notification for an S3 bucketthat holds sensitive audit data of a firm. As the Team lead, you are receiving the SNS notifications for every evnet in this bucket. After validating the event data, you realizedd that few events are missing. What could be the reason for this behavior and how to avoid this in the future ?  

**Đúng**
- If two writes are made to a single non-versioned object at the same time, it is possible that only a single event notification will be sent

**Sai**
- Versioning is enabled on the S3 bucket and event notifications are getting fired for only one version
- Someone could have created a new notification configuration and that has overridden your existing configuration
- Your notification action is writing to the same bucket that triggers the notification

**Giải thích**

Nếu 2 writes được làm cho 1 đối tượng signle và không bật versioning tại cùng 1 thời điểm, nó có thể chỉ 1 single event notification sẽ được gửi 

## Câu 20: The dev team at an IT company uses CloudFormation to manage its AWS infrastructure. The team has created a network stack containing a VPC with subnets and a web application stack with EC2 instances and an RDS instances. The team wants to reference the VPC created in the network stack into its web application stack. As a DVA, which of the following solutions would you recommend for the given use-case ? 

**Đúng**
- Create a cross-stack reference and use the Export output field to flag the value of VPC from the network stack. Then use Fn::ImportValue intrinsic function to import the value of VPC into the web application stack

**Sai**
- Create a cross-stack reference and use the Outputs output field to flag the value of VPC from the network stack. Then use Fn::ImportValue intrinsic function to import the value of VPC into the web application stack
- Create a cross-stack reference and use the Outputs output field to flag the value of VPC from the network stack. Then use Ref intrinsic function to reference the value of VPC into the web application stack
- Create a cross-stack reference and use the Export output field to flag the value of VPC from the network stack. Then use Ref intrinsic function to reference the value of VPC into the web application stack

**Giải thích**



## Câu 21: a dev is creating a RESTful API service using an API Gateway with AWS Lambda integration. The service must support different API versions for testing purposes. As dva, which of the following would you suggest as the best way to accomplish this ? 

**Đúng**
- Deploy the API versions as unique stages with unique endpoints and use stage variables to provide the context to identify the API versions

**Sai**
- Use an X-Version header to identify which version is being called and pass that header to the Lambda function
- Use an API Gateway Lambda authorizer to route API clients to the correct API version
- Set up an API Gateway resource policy to identify the API versions and provide context to the Lambda function

**Giải thích**

Nên quản lý version theo stage, mỗi stage có 1 endpoint riêng. Mỗi stage đều có Stage Variable là biến config theo từng stage, lambda đọc biến này để biết version

Flow chuẩn AWS: Client -> API Gateway ( stage v1/v2 ) -> Lambda -> stage variable ( version )

## Câu 22: An e-commerce application writes log file into S3. The application alse reads these log files in parallel on a near real-time. The dev team wants to address any data discrepancies that might arise when application overwrites an existing log file and then tries to read that specific log file. Which of the following options BEST describes the capabilities of S3 relevant to this scenario ? 

**Đúng**
- A process replaces an existing object and immediately tries to read it. Amazon S3 always returns the latest version of the object

**Sai**
- A process replaces an existing object and immediately tries to read it. Until the change is fully propagated, Amazon S3 might return the previous data
- A process replaces an existing object and immediately tries to read it. Until the change is fully propagated, Amazon S3 does not return any data
- A process replaces an existing object and immediately tries to read it. Until the change is fully propagated, Amazon S3 might return the new data

**Giải thích**

App ghi log vào S3 và có nhiều read song song gần như real-time. Có thể có overwrite file và read ngay lập tức. Lúc này, S3 luôn luôn trả về phiên bản mới nhất của object

## Câu 23: A video streaming application uses AWS CloudFront for its data distribution. The dev team has decided to use CloudFront with origin failover for HA. Which of the following options are correct while configuring CloudFront with Origin Groups ( select two options )

**Đúng**
- CloudFront fails over to the secondary origin only when the HTTP method of the viewer request is GET, HEAD or OPTIONS
- CloudFront routes all incoming requests to the primary origin, even when a previous request failed over to the secondary origin

**Sai**
- When there’s a cache hit, CloudFront routes the request to the primary origin in the origin group
- To set up origin failover, you must have a distribution with at least three origins
- In the Origin Group of your distribution, all the origins are defined as primary for automatic failover in case an origin fails

**Giải thích**

CloudFront chỉ failover cho các HTTP method là: GET, HEAD, OPTIONS

Routing Behavior: 
- CloudFront luôn route request đến primary origin trước, ngay cả khi request trước đó đã failover
- Chỉ gửi request đến secondary origin sau khi primary origin fail
- Response từ secondary origin có hành vi giống như origin thông thường 

## Câu 24: You are a dev working at a cloud company that embraces serverless. You have performed your initial deployment and would like to work towards adding API gateway stages and asssociate them with existing deployments. Your stages will include prod, test and dev and will need to match a Lambbda function variant that can be updated overtime. Which of the following features must you add to achieve this ? 

**Đúng**
- Lambda Aliases
- Stage Variables

**Sai**
- Lambda X-Ray integration
- Mapping Templates
- Lambda Versions

**Giải thích**

Yêu cầu cần tạo API gateway stages ( prod, test, dev). Liên kết các stage với existing deployments. Mỗi stage cần match với Lambda function variant. Lambda function có thể được update theo thời gian. 

Lambda Aliases là con trỏ có thể thay đổi trỏ đến các Lambda versions cụ thể
```
{
  "prod": "points to version 3",
  "test": "points to version 2", 
  "dev": "points to version $LATEST"
}
```
Stage Variables là key-value pairs được định nghĩa ở stage level
```
{
  "stage": "prod",
  "variables": {
    "lambdaAlias": "prod",
    "environment": "production"
  }
}
```

## Câu 25: a retail company manages its IT infra on AWS via Elastic Beanstalk. The dev team at the company is planning to deploy the next version with MINIMUM application downtime and the ability to rollback quickly in case deployment goes wrong. 

**Đúng**
- Deploy the new version to a separate environment via Blue/Green Deployment, and then swap Route 53 records of the two environments to redirect traffic to the new version

**Sai**
- Deploy the new application version using 'All at once' deployment policy
- Deploy the new application version using 'Rolling with additional batch' deployment policy
- Deploy the new application version using 'Rolling' deployment policy

**Giải thích**

Công ty cần MINIMUM application downtime. Ability to rollback quickly. Trong trường hợp này thì Blue/Green Deployment nó đảm bảo
- deploy  1 version riêng biệt
- có thể test trực tiếp trên env mới mà không ảnh hưởng env cũ. Cùng với đó có thể thực thi Canary Switch traffic
- Swap Route53 records để redirect traffic từ blue -> green
- Rollback bằng swap lại là xong. Vừa đơn giản vừa nhanh, tùy vào TTL của Route53 records

## Câu 26: A dev team has created AWS CloudFormation templates that are reusable by taking advantage of input parameters to name resource based on client names. 

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


