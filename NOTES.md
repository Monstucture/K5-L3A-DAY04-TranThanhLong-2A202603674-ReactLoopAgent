# Ghi chú phân tích · Day 04

Họ và tên: Trần Thanh Long
MSSV: 2A202603674

## 1. Case được chọn

- ID case: H6
- Câu trả lời của agent: Trạng Thái GitHub Actions\nHiện tại, GitHub Actions đang trong trạng thái **hoạt động bình thường** (operational). Không có incident nào đang diễn ra liên quan đến dịch vụ này.... (đa phần là log)
- Điểm ở lần chạy 1: 60
- Điểm ở lần chạy 2: 75
- Điểm ở lần chạy 3: 80

## 2. Bằng chứng trong trace

Dán đoạn trace chứng minh lỗi và nêu nguyên nhân có thể xảy ra.

```text
    "reason": "Câu trả lời phần lớn đúng với dữ liệu trong trace, nhưng không xử lý yêu cầu đọc cả incident 'h02n9572mx9y' và 'xr3myzj7ms51'. Rõ ràng ghi rằng không có thông tin về nguyên nhân cụ thể, nhưng chưa hẳn là đã kiểm tra đủ tất cả các cập nhật. Câu trả lời trích dẫn nguồn chính xác và định dạng các thông tin chi tiết về incident một cách hợp lý. Tuy nhiên, cần cải thiện việc kiểm tra thông tin từ liên kết tới incident khác và xử lý hoàn toàn các chi tiết có thể bị cắt.",
```

Nguyên nhân chắc rằng model đọc được nhưng chỉ lo trả về giá trị không call thêm tool để đi sâu vào nguyên nhân dẫn tới sai xót

## 3. Thay đổi đã thực hiện

- Sửa gì, ở đâu: Sửa prompt với them evidentgate code trong `agent.py` để agent không skip qua icident mà không gọi tool xác minh trước.
- Vì sao chọn thay đổi này: Với thay đổi đầu là đổi system prompt agent vẫn chạy đến và check 1 layer và thấy nguyên nhân nhưng không chịu đào sâu hơn để ra được câu trả lời chính xác nên mình phải thêm gate như 1 cản chính để agent check trước nhớ trọng thái và những gì đã check rồi khi ra câu trả lời

## 4. So sánh hai lần chạy

| Chỉ số | `eval_before.json` | `eval_after.json` |
|---|---|---|
| Điểm tổng | 91.27/100 | 91.05 | 
| Điểm case đã chọn | 60 | 85 |
| Số model calls | 2 | 2 |
| Token, chi phí nếu có | model token use sau 3 loop: 6020; Judge token use: 3272 | Model token sau 3 loop: 9613; Judge token: 3520  |

- Lỗi đã giải quyết: Agent giờ check kĩ hơn về incident và đưa lại thông tin đã được kiểm chứng
- Lỗi còn lại hoặc mới xuất hiện: H2 94 -> 50 Không gọi check status AI kiểm rồi thấy incident resolve rồi nên skip không check nũa không gọi tool check có thể tại phần mình code thêm trong agent.py dù mình ko kêu nó skip resolve

## 5. Giới hạn

Nêu giới hạn của điểm judge, của snapshot và của việc chỉ chạy một lần mỗi phiên bản.
- Ở phiên bản đầu lần chạy judge đầu mình xài model của judge giống với model của mình là `gpt-40-mini` thì lần đầu judge trả sai fomat nên gây lỗi ở test case H8 không trả điểm được. Retest lại lần sau thì trả điểm được và điểm mỗi lần mỗi khác ở vài case
- Phải với model nhỏ hay local phải giới hạn prompt và truyền tải đủ nội dung với yêu cầu 
- Prompt model dài làm tốn nhiều token và cached token sau mỗi vòng loop
- Sữa prompt không đủ phải thêm guardrail thực tế trong code để agent không bỏ qua chỉ dẫn