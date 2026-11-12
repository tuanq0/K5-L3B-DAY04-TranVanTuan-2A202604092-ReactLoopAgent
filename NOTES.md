# Ghi chú phân tích · Day 04

Họ và tên: Trần Văn Tuấn
MSSV:2A202604092

## 1. Case được chọn

- ID case: H8
- Câu trả lời của agent: 

"answer": "Tôi không thể liệt kê đầy đủ incident của GitHub và Cloudflare trong giai đoạn 2020–2026. Công cụ hiện có không cho phép làm việc này.\n\n## Giới hạn của dữ liệu\n\n- `get_status` chỉ trả về một lát cắt incident gần đây. Ghi chú của công cụ nói rõ nó chỉ tìm trong 50 incident gần nhất.\n- Kết quả chỉ hiển thị 5 incident cho mỗi nhà cung cấp, dù ghi chú nhắc tới 50 incident gần nhất.\n- `get_incident_details` chỉ đọc chi tiết khi đã biết ID. Nó không có chức năng duyệt hay phân trang theo năm.\n- Hai công cụ không có tham số lọc theo khoảng thời gian, nên tôi không truy vấn được 2020–2025 hay các tháng trước 11/2026.\n- Tôi cũng không thể liệt kê \"mọi component\". Trường `components` trả về rỗng ở cả hai nhà cung cấp.\n- Việc không thấy một incident trong kết quả không chứng minh incident đó không tồn tại.\n\n## Những gì tôi xác nhận được\n\nSnapshot lúc 2026-11-09 03:50 UTC. Các dòng dưới đây chỉ là danh sách incident gần đây, không phải toàn bộ lịch sử.\n\n### GitHub\nNguồn: githubstatus.com. Trạng thái tổng thể là \"All Systems Operational\".\n\n| ID | Tên | Trạng thái | Cập nhật cuối |\n|---|---|---|---|\n| pyynvrm1vskw | Increase in timeouts for some unauthenticated users | resolved | 2026-11-08 12:32 |\n| pfbmpxxfq1cy | Degraded performance for multiple GitHub services | resolved | 2026-11-06 00:20 |\n| t89g2q1800fr | Incident with Actions | resolved | 2026-11-05 17:42 |\n| h02n9572mx9y | Incident with Actions | resolved | 2026-11-07 01:10 |\n| xr3myzj7ms51 | Incident with Actions and Pages | resolved | 2026-11-07 01:04 |\n\n### Cloudflare\nNguồn: cloudflarestatus.com. Trạng thái tổng thể là \"Partial System Outage\", tức là đang có sự cố.\n\n| ID | Tên | Trạng thái | Cập nhật cuối |\n|---|---|---|---|\n| x9orajjmvvwa | Network Performance Issues in Chicago | resolved | 2026-11-07 05:35 |\n| c60k3g4vsoc1 | Spectrum Availability Issue in Phoenix | **monitoring** (chưa đóng) | 2026-11-07 02:51 |\n| wp49bp8bqo6c | Fedramp Customers have Delayed Purges | **identified** (chưa đóng) | 2026-11-06 17:59 |\n| 3ehw20ozwlkr | Network Performance Issues in Singapore | resolved | 2026-11-06 10:43 |\n| wtsd415cavkc | Wrangler Deployments, Pages, and Workers Builds failures | resolved | 2026-11-06 10:08 |\n\nHai incident Cloudflare `c60k3g4vsoc1` (Phoenix) và `wp49bp8bqo6c` (Fedramp) chưa được đóng. Phạm vi của chúng giới hạn theo địa lý hoặc nhóm khách hàng. Tôi chưa đọc chi tiết, nên chưa kết luận được chúng có ảnh hưởng đến một người dùng cụ thể hay không.\n\n## Cách lấy danh sách đầy đủ\n\n- Dùng trang lịch sử chính thức: githubstatus.com/history và cloudflarestatus.com/history.\n- Dùng API Statuspage (`/api/v2/incidents.json`) và phân trang theo năm.\n- Với GitHub, có thể dùng thêm RSS/Atom feed của trang trạng thái.\n\nTôi có thể đọc chi tiết từng incident ở trên, gồm timeline và phạm vi, nếu bạn muốn. Nếu bạn có danh sách ID từ nguồn khác, tôi cũng có thể kiểm tra các ID đó.",

- Điểm ở lần chạy 1:89

## 2. Bằng chứng trong trace

Dán đoạn trace chứng minh lỗi và nêu nguyên nhân có thể xảy ra.

"improvement": "Nêu rõ hơn rằng các nhận định về công cụ (không có tham số lọc, không phân trang) là suy ra từ kết quả quan sát; tránh suy diễn phạm vi incident từ tên; ngắn gọn hơn và nhấn mạnh sớm giới hạn 50 incident gần nhất/provider."

## 3. Thay đổi đã thực hiện

- Sửa gì, ở đâu: system prompt, thêm tránh suy diễn phạm vi incident từ tên

## 4. So sánh hai lần chạy

| Chỉ số | `eval_before.json` | `eval_after.json` |
|---|---|---|
| Điểm tổng | 94.18 | 95.18 |
| Điểm case đã chọn | 89 | 91 |
| Số model calls | 1 | 1 |
| Token, chi phí nếu có |  |  |

- Lỗi đã giải quyết:
- Lỗi còn lại hoặc mới xuất hiện:

## 5. Giới hạn

- LLM judge chỉ là đánh giá xấp xỉ; nó có thể không đồng ý với cách diễn giải dữ liệu kỹ thuật và có thể chấm điểm cao hoặc thấp bất thường dù câu trả lời gần đúng.
- Snapshot là dữ liệu chốt ở một thời điểm cụ thể; nó không khớp với trạng thái thực tế sau thời điểm đó và không thể dùng để khẳng định sự cố hiện tại nếu đã qua lúc snapshot.
- Chỉ chạy một lần mỗi phiên bản là chưa đủ để kết luận độ ổn định: cùng một prompt nhưng model/provider/tool có thể cho kết quả khác nhau giữa các lần.
- Với các case về status page, giới hạn quan trọng là: incident được liệt kê trong snapshot chỉ là “incident gần đây” theo 50 item, không phải toàn bộ lịch sử. Vì vậy, cách an toàn nhất là nói “không đủ bằng chứng”, “không tìm thấy trong phạm vi dữ liệu” chứ không nói “không có”.
