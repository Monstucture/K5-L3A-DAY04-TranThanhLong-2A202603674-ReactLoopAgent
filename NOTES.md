# Ghi chú phân tích · Day 04

Họ và tên: Trần Thanh Long
MSSV: 2A202603674

## 1. Case được chọn

- ID case: H6
- Câu trả lời của agent: Trạng Thái GitHub Actions\nHiện tại, GitHub Actions đang trong trạng thái **hoạt động bình thường** (operational). Không có incident nào đang diễn ra liên quan đến dịch vụ này.... (đa phần là log)
- Điểm ở lần chạy 1: 60

## 2. Bằng chứng trong trace

Dán đoạn trace chứng minh lỗi và nêu nguyên nhân có thể xảy ra.

```text
    "reason": "Câu trả lời phần lớn đúng với dữ liệu trong trace, nhưng không xử lý yêu cầu đọc cả incident 'h02n9572mx9y' và 'xr3myzj7ms51'. Rõ ràng ghi rằng không có thông tin về nguyên nhân cụ thể, nhưng chưa hẳn là đã kiểm tra đủ tất cả các cập nhật. Câu trả lời trích dẫn nguồn chính xác và định dạng các thông tin chi tiết về incident một cách hợp lý. Tuy nhiên, cần cải thiện việc kiểm tra thông tin từ liên kết tới incident khác và xử lý hoàn toàn các chi tiết có thể bị cắt.",
```

Nguyên nhân chắc rằng model đọc được nhưng chỉ lo trả về giá trị không call thêm tool để đi sâu vào nguyên nhân dẫn tới sai xót

## 3. Thay đổi đã thực hiện

- Sửa gì, ở đâu: 
- Vì sao chọn thay đổi này:

## 4. So sánh hai lần chạy

| Chỉ số | `eval_before.json` | `eval_after.json` |
|---|---|---|
| Điểm tổng | 91.27/100 | | 
| Điểm case đã chọn | 60 |  |
| Số model calls | 2 |  |
| Token, chi phí nếu có | model token use: 342; Judge token use: 3272 |  |

- Lỗi đã giải quyết: 
- Lỗi còn lại hoặc mới xuất hiện:

## 5. Giới hạn

Nêu giới hạn của điểm judge, của snapshot và của việc chỉ chạy một lần mỗi phiên bản.
- Ở phiên bản đầu lần chạy judge đầu mình xài model của judge giống với model của mình là `gpt-40-mini` thì lần đầu judge trả sai fomat nên gây lỗi ở test case H8 không trả điểm được. Retest lại lần sau thì trả điểm được và điểm mỗi lần mỗi khác ở vài case

