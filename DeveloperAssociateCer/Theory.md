<!-- # Message Service

## SQS

## SNS

# Kinesis services

## Kinesis data stream 

## Kinesis data firehose

## Amazon Managed Service for Apache Flink -->

# Monitoring services

## Cloudwatch Metrics

- Cloudwatch cung cấp metrcis cho nhiều dich vụ trong AWS
- Metric là biến để giám sát (CPUUtilization, NetweorkIn)
- Metrics thuộc về 1 namespace
- Dimension là 1 thuộc tính của metric ( instanceid, environment, etc...)
- Tối đa 30 dimensions trên 1 metric
- Metrics có timestamps
- Có thể tạo CloudWatch dashboards cho metrics

## Cloudwatch Custom Metrics

- Có thể định nghĩa và gửi custom metric của bạn lên CloudWatch
- Ví dụ: Memory usage, disk space, sso log trong users...
- Sử dụng API call **PutMetricData**
- Có thể sử dụng dimensions ( thuộc tính ) để phân loại metrics
    - Instance.id
    - Environment.name
- Metric resolution ( StorageResolution API Paramater - có 2 giá trị)
    - Standard: 1 phút ( 60 giây )
    - High Resolution: 1/5/10/30 giây - chi phí cao hơn 
- **Quan trọng**: Chấp nhận metric data point trong 2 tuần trước và 2 tiếng trong tương lai ( đảm bảo rằng cấu hình EC2 instance của bạn là hợp lệ )

## CloudWatch Logs

- Log groups: Tên tùy chỉnh, thường đại diện cho 1 Application
- Log stream: instance trong ứng dụng / log files / containers 
- Có thể định nghĩa  **Log expiration policies**: không bao giờ hết hạn, 1 ngày, 10 năm,...
- CloudWatch Logs có thể gửi Logs tới :
    - Amazon S3 (exports)
    - Kinesis Data Streams
    - Kinesis Data Firehose
    - AWS Lambda
    - OpenSearch
- Logs được mã hóa mặc định
- Có thể setup mã hóa dựa vào KMS với Key của bạn 

## CloudWatch Logs - Sources

- SDK, CloudWatch Logs Agent, CloudWatch Unified Agent
- Elastic Beanstalk: sưu tập logs từ application
- ECS: sưu tập logs từ containers
- AWS Lambda: sưu tập từ function logs
- VPC Flow Logs: VPC chỉ định logs
- API Gateway
- Logs dựa vào bộ lọc của CloudTrail
- Route53: Log DNS queries

#### CloudWatch Logs Agent

- Là một chương trình chạy trên EC2 instance để push log lên CloudWatch
- Theo mặc định, không có log từ EC2 nào tới CloudWatch
- Đảm bảo EC2 có IAM Permissions hợp lệ
- CloudWatch log agent cũng có thể được setup ở on-premises

#### CloudWatch Unified Agent

- Dùng cho virutal server như EC2 instance, on-premises server,...
- CloudWatch LogsAgent
    - Phiên bản cũ của Agent
    - Chỉ có thể gửi CloudWatch Logs
- CloudWatch Unified Agent 
    - Sưu tập số liệu thêm ở mức hệ thống chẳng hạn như RAM, processes,...
    - Sưu tập log để gửi tới CloudWatch Logs
    - Tập trung hóa cấu hình sử dụng SSM Parameter Store

####