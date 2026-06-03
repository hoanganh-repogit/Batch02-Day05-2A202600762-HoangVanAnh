# Template — Thin SPEC Cuối Day 05

Thin SPEC không phải PRD đầy đủ. Đây là bản cam kết đủ rõ để sáng Day 06 nhóm build ngay.

## 1. Track, product/app và user

**Track:** Travel & Hospitality  
**Product/app thật:** Vinpearl  
**User cụ thể:** Khách gia đình tìm phòng (có kèm trẻ em) và Khách đang lưu trú tại resort cần hỏi thông tin.  
**Nhóm có phải user thật không? Nếu không, khác ở đâu?** Không hoàn toàn. Nhóm có chuyên môn tech nên khi chat thường dùng từ khóa rõ ràng, trong khi user thật thường dùng ngôn ngữ đời thường, mơ hồ và hay sai lỗi chính tả.

## 2. Evidence summary

| Evidence | Nguồn | User/pain nói lên điều gì? | SPEC phải đổi gì? |
|---|---|---|---|
| Bot không bóc tách được điều kiện số lượng và độ tuổi trẻ em để lọc phòng. | Tự test (Self-use) | User mệt mỏi khi phải chat dài dòng nhưng bot vẫn bắt tự click tay. | Thay vì ép đặt phòng bằng chat, dùng AI để bóc tách text (Augment) điền form filter. |
| Hỏi tiện ích nội khu (hồ bơi) thì bot trả lời chung chung, không có context. | Review / Tự test | User ức chế vì bot vô cảm, không biết khách đang ở khu nào. | Làm tính năng AI Concierge, dùng thông tin booking hiện tại làm context trả lời. |
| Yêu cầu đổi ngày đặt phòng qua chat bị từ chối/bắt gọi tổng đài. | Tự test (Self-use) | Luồng nghiệp vụ rủi ro cao khiến bot không dám xử lý. | Đưa luồng này vào Failure Path, xử lý bằng Fallback Deep link. |

## 3. Pain statement

```text
User [Khách gia đình hoặc khách đang lưu trú] đang gặp khó ở [bước lọc tìm phòng phức tạp và tra cứu tiện ích nội khu],
vì [chatbot hiện tại không bóc tách được yêu cầu chi tiết (độ tuổi trẻ em) và thiếu ngữ cảnh (không biết khách ở đâu)],
dẫn tới [hậu quả là tốn thời gian, luồng giao tiếp đứt gãy vì bot liên tục quăng link bắt tự đọc].
Bằng chứng chính là [self-test cho thấy bot báo không hiểu hoặc trả link chung chung khi gặp câu hỏi ghép nhiều điều kiện].
```

## 4. Build slice

```text
Cho [khách hàng] đang [tìm phòng hoặc cần tư vấn tiện ích resort],
prototype sẽ dùng AI để [augment hành động hẹp: bóc tách thông tin chat để điền sẵn form filter, và tự động trả lời câu hỏi dựa trên booking có sẵn],
tạo ra [output là một bảng form tìm kiếm đã được điền sẵn hoặc câu trả lời Q&A đúng context],
và xử lý [failure mode (vd yêu cầu thay đổi lịch rủi ro cao)] bằng [mitigation: cung cấp Deep link điều hướng tới màn hình quản lý Native].
```

## 5. Auto/Aug decision

Chọn một:

- [x] **Augmentation:** AI gợi ý/draft/phân loại, user quyết cuối.
- [ ] **Conditional automation:** AI tự làm trong case hẹp; case mơ hồ/rủi ro chuyển người.
- [ ] **Automation:** AI tự quyết và tự hành động.

**Lý do chọn:** Việc đặt sai phòng hoặc thanh toán nhầm gây rủi ro tài chính cao và UX rất tệ nếu chỉ dùng text. AI chỉ nên điền sẵn form giúp user (Augment), user vẫn là người trực tiếp bấm "Tìm kiếm" / "Thanh toán".  
**Human role:** Decider (User là người chốt quyết định).

## 6. Four paths

| Path | Prototype phải thể hiện gì? |
|---|---|
| Happy | AI trích xuất đúng ngày, số người, độ tuổi trẻ em và điền vào form UI. Hoặc trả lời đúng giờ mở cửa hồ bơi ở khu resort khách đang ở. |
| Low-confidence | Khách gõ "đi cùng con nhỏ", AI không rõ độ tuổi nên hỏi lại: "Dạ bé nhà mình năm nay mấy tuổi để em tìm phòng phù hợp ạ?". |
| Failure | Khách yêu cầu: "Hủy phòng tuần sau đi". AI nhận diện rủi ro cao, không tự xử lý mà hiển thị: "Việc hủy phòng có thể phát sinh phí, bạn thao tác trực tiếp tại [Link Quản lý Booking] nhé". |
| Correction | Khách bảo: "Không, đi 3 người lớn chứ không phải 2 lớn 1 nhỏ". AI nhận diện đính chính, tự động cập nhật lại biến (variables) và fill lại form UI ngay lập tức. |

## 7. Failure mode nguy hiểm nhất

```text
Nếu user [yêu cầu thay đổi ngày đi, hủy phòng, hoặc thanh toán trực tiếp qua chat],
AI có thể [failure: hiểu sai ý, thực hiện lệnh sai dẫn đến mất tiền hoặc mất phòng],
hậu quả là [impact: tổn thất tài chính cho user và khủng hoảng CSKH].
Prototype sẽ xử lý bằng [fallback: nhận diện intent rủi ro cao và đưa Deep link chuyển user về màn hình quản lý Native của app].
Owner kiểm thử path này là [Thành viên A - Nhóm tự điền].
```

## 8. Owner plan cho sáng Day 06

| Thành viên | Việc phụ trách | Bằng chứng cần có trong repo |
|---|---|---|
| [Thành viên 1] | Research / evidence | Bổ sung link ảnh test Layla AI / Screenshot App. |
| [Thành viên 2] | SPEC | Hoàn thiện file Thin SPEC & cập nhật quyết định. |
| [Thành viên 3] | Prototype | Bot cơ bản có thể extract entity (Ngày, Người, Tuổi). |
| [Thành viên 4] | Test / failure path | Trigger thử luồng Hủy phòng để ép fallback. |
| [Thành viên 5] | Demo script / repo | Viết kịch bản demo 3 phút (mở đầu, happy path, failure). |
