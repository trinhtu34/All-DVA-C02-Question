# Về vấn đề tư duy trong việc giải quyết vấn đề khi có sự cố bảo mật

#### Chiến lược điều tra và phân tích

Nên làm theo kiểu hiểu tư duy của đội nhóm trước khi review action của họ. Khi có 1 vấn đề xảy ra thì không nên nhảy thẳng vào làm gì luôn, mà phải hiểu tư duy, lối tiếp cận vấn đề của team Security trước khi lên kế hoạch làm gì. 

Phải biết được, thể hiện được là mình tư duy ra sao, team Sec tư duy ra sao, lập kế hoạch thế nào. Nếu không biết tư duy giải quyết, hướng tiếp cận vấn đề thì như là làm 1 bài toán đưa kết quả nhưng không có lời giải làm sao để có kết quả đó. 

#### Hướng dẫn điều tra - gồm 4 phần 

1. Phạm vi thông tin xác thực có thể bị đánh cắp. Access key, credentials, OIDC Token, ... nào lưu trong hệ thống. Đếm hết và audit lần sử dụng cuối là khi nào
2. Soi log network và Monitoring. Xem Logs của Network xem có lưu lượng mạng lạ ra ngoài không, check xem có hành vi bất thường nào không, check xem hệ thống có đang expose ra port nào lạ không
3. Soi các log action ví dụ như dùng CloudTrail để xem có Resources nào bị tạo ngoài kế hoạch hay không. Đây là dấu vết kinh điển của Attacker khi chúng có được các credentials.
4. Kiểm tra cả ở các nơi mà ít khi động tới, ví dụ như ở AWS thì kiểm tra xe có Resources nào tạo ở các region mà không có trong kế hoạch hay không, Attacker thường sẽ tạo các Resources ở những vị trí, region mình ít khi hoặc chưa bao giờ sử dụng.

#### Hướng dẫn xử lý

1. Rotate toàn bộ key. Nhưng chú ý không Rotate các key không liên quan, vì có khả năng làm Down Services khi update key.
2. Nếu dùng Container Images thì chú ý chặn các images chứa trojan hoặc ransomware bị nhiễm trong Registry, Không xóa. Vì có thể sau này team Security sẽ cần audit và điều tra ngược để truy vết cuộc tấn công.

Cách làm: gắn tag cho các resource có vấn đề, sau đó tạo các Policy, nếu dùng ECR thì dùng Resource Policy chặn mọi hành động pull (lệnh BatchGetimage) đến những images có tag nguy hiểm. Images vẫn nằm đó nhưng không ai kéo về.