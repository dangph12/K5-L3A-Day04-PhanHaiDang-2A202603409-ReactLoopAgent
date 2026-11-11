# Ghi chú phân tích · Day 04

Họ và tên: Phan Hải Đăng
MSSV: 2A202603409

## 1. Case được chọn

- ID case: A5
- Câu trả lời của agent: Hiện tại, GitHub không có các sự cố (incident) liên quan đến GitHub Actions trong năm 2025. Tại thời điểm tôi tìm kiếm, tất cả các sự cố gần đây nhất có liên quan chỉ xảy ra vào năm 2026, và tất cả đều đã được giải quyết.\n\nDanh sách các sự cố liên quan đến GitHub Actions trong năm 2026:\n1. **Incident with Actions**\n - **Thời gian:** 2026-11-05\n - **Chi tiết:** Đã giải quyết, liên quan đến độ trễ hiệu suất.\n - [Link chi tiết](https://stspg.io/lv4q3xm9r0rk)\n\n2. **Incident with Actions and Pages**\n - **Thời gian:** 2026-11-04\n - **Chi tiết:** Đã giải quyết, liên quan đến độ trễ trong quy trình công việc của Actions và Pages.\n - [Link chi tiết](https://stspg.io/pjgptg3cfc6n)\n\n3. **Delayed Actions status check results**\n - **Thời gian:** 2026-10-23\n - **Chi tiết:** Đã giải quyết, liên quan đến việc cập nhật trạng thái của Actions bị trễ.\n - [Link chi tiết](https://stspg.io/kmk42jg3v1sm)\n\n4. **Incident with Actions (thêm thông tin)**\n - **Thời gian:** 2026-10-22\n - **Chi tiết:** Đã giải quyết, có vấn đề trong việc xử lý hiện tại của các công việc Actions.\n - [Link chi tiết](https://stspg.io/44w94j036yc8)\n\n5. **Degraded availability for GitHub Actions, GitHub Pages and Webhooks**\n - **Thời gian:** 2026-10-19\n - **Chi tiết:** Đã giải quyết, có độ trễ trong việc xử lý công việc và thông báo.\n - [Link chi tiết](https://stspg.io/nrv2k8s68bwn)\n\n**Lưu ý:** Chỉ các sự cố trong năm 2026 đang được xem xét và không có thông tin nào cho năm 2025. Dữ liệu trên được lấy từ trang trạng thái chính thức của GitHub [tại đây](https://www.githubstatus.com), và bạn có thể thấy rằng có những hạn chế trong việc tiếp cận lịch sử sự cố trước năm 2026.
- Điểm ở lần chạy 1: 45

Nhận định mở đầu rằng năm 2025 không có sự cố nào liên quan đến GitHub Actions là không có bằng chứng hỗ trợ. Câu hỏi yêu cầu danh sách đầy đủ các sự cố trong năm 2025, nhưng dữ liệu snapshot hiện có không thể cung cấp danh sách đó.

## 2. Bằng chứng trong trace

Trace cho thấy lỗi:

```text
"result": {
  "provider": "github",
  "captured_at": "2026-11-09T03:50:47.065020+00:00",
  "overall": {
    "indicator": "none",
    "description": "All Systems Operational"
  },
  "components": [],
  "incidents": [
    {
      "id": "t89g2q1800fr",
      "name": "Incident with Actions",
      "status": "resolved",
      "updated_at": "2026-11-05T17:42:35.455Z"
    },
    {
      "id": "xr3myzj7ms51",
      "name": "Incident with Actions and Pages",
      "status": "resolved",
      "updated_at": "2026-11-07T01:04:38.647Z"
    },
    {
      "id": "ntrfd2jqldn6",
      "name": "Delayed Actions status check results",
      "status": "resolved",
      "updated_at": "2026-10-28T22:29:49.032Z"
    },
    {
      "id": "q29p76qtnkyk",
      "name": "Incident with Actions",
      "status": "resolved",
      "updated_at": "2026-11-05T00:04:42.627Z"
    },
    {
      "id": "8tzbpcnl75jp",
      "name": "Degraded availability for GitHub Actions, GitHub Pages and Webhooks",
      "status": "resolved",
      "updated_at": "2026-10-22T13:12:51.444Z"
    }
  ],
  "matching_incidents_in_last_50": 11,
  "note": "Chỉ tìm theo chuỗi trong 50 incident gần nhất; không tìm thấy không chứng minh không có sự cố.",
  "source": "https://www.githubstatus.com"
```

Trace cho thấy công cụ chỉ tìm trong 50 sự cố gần đây nhất. Snapshot được ghi nhận vào tháng 11/2026 và không bao phủ đầy đủ lịch sử năm 2025. Vì vậy, dữ liệu này không đủ để kết luận năm 2025 không có sự cố.

## 3. Thay đổi đã thực hiện

- Trong `agent.py`, system prompt nêu rõ giới hạn 50 sự cố và yêu cầu không khẳng định dữ liệu bao quát toàn bộ lịch sử hoặc kết luận không có sự cố cũ hơn. Bổ sung vào system prompt giới hạn tìm kiếm trong 50 sự cố gần nhất.

## 4. So sánh hai lần chạy

| Chỉ số                | `eval_before.json` | `eval_after.json` |
| --------------------- | ------------------ | ----------------- |
| Điểm tổng             | 80.91              | 80.68             |
| Điểm case đã chọn     | 45                 | 65                |
| Số model calls        | 52                 | 47                |
| Token, chi phí nếu có | 46.880             | 39.268            |

- Lỗi đã giải quyết: Case A5 tăng từ 45 lên 65 điểm; số lượt gọi model giảm từ 4 xuống 2.
- Lỗi còn lại hoặc mới xuất hiện: Điểm tổng giảm nhẹ từ 80.91 xuống 80.68; E3, A8 và H2 cũng giảm điểm. Cần chạy nhiều lần để xác định độ ổn định.

## 5. Giới hạn

- Điểm do judge chấm có thể biến động và không phản ánh đầy đủ mọi khía cạnh về tính đúng đắn; cần đọc câu trả lời và trace để hiểu nguyên nhân điểm.
- Snapshot chỉ ghi nhận dữ liệu tại thời điểm lấy mẫu và công cụ tìm kiếm bị giới hạn ở 50 sự cố gần nhất. Do đó, kết quả không chứng minh được danh sách lịch sử đầy đủ hoặc việc không có sự cố cũ hơn.
- Mỗi phiên bản mới chỉ chạy một lần thì chưa đủ để kết luận cải tiến ổn định hay điểm giảm là do thay đổi trong `agent.py`; kết quả có thể chịu ảnh hưởng của độ biến động từ model hoặc judge.
