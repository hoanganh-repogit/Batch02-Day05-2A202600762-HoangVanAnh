# Template — Evidence Pack

Nộp kèm thin SPEC cuối Day 05.

## 1. Nhóm và track

- **Tên nhóm:** Ngũ Hổ Tướng.
- **Track:** Travel & Hospitality.
- **Product/app đã chọn:** Vinpearl.
- **Build slice đang nghĩ:** Booking & AI Concierge (Trợ lý ảo hỗ trợ đặt phòng và lưu trú).

## 2. Self-use evidence

Nhóm tự dùng app/workflow và ghi lại điểm gãy.

| Observation | Screenshot / link | Path liên quan | Điều học được |
|---|---|---|---|
| Gõ: "Tìm phòng cho gia đình 4 người, có trẻ em 5 tuổi ở Phú Quốc" -> Bot trả link chung chung, không tự điền filter số người/trẻ em. |  https://layla.ai/en/chat/01KT6F4S2AFEM00EZHM4TY75X9/trip/01KT6F7YATZ47A4V7N5XFC1PXY | Failure | Bot chưa bóc tách (extract) được entity phức tạp để chuyển thành bộ lọc (filter) trên UI. Cần kết hợp Chat + UI. |
| Hỏi: "Resort đang ở có khu vui chơi cho bé không?" -> Giới hạn lượt hỏi. |  https://layla.ai/en/chat/01KT6F4S2AFEM00EZHM4TY75X9/trip/01KT6F7YATZ47A4V7N5XFC1PXYhttps://layla.ai/en/chat/01KT6FC1ESXRR5D1MSXTX707Y9/trip/01KT6F7YATZ47A4V7N5XFC1PXY | Happy | Bot làm rất tốt mảng Q&A thông tin tĩnh và up-sell dịch vụ. |
| Yêu cầu: "Đổi ngày đặt phòng cho booking XYZ" -> Bot báo không hiểu hoặc yêu cầu gọi Hotline. |  https://layla.ai/en/chat/01KT6F4S2AFEM00EZHM4TY75X9/trip/01KT6F7YATZ47A4V7N5XFC1PXYhttps://layla.ai/en/chat/01KT6FC1ESXRR5D1MSXTX707Y9/trip/01KT6F7YATZ47A4V7N5XFC1PXY | Low-confidence / Failure | Flow thay đổi/hủy booking có risk cao, bot chưa dám xử lý. Cần human-handoff hoặc Deep link rõ ràng đến trang quản lý booking. |

## 3. User / review / social evidence

Nguồn có thể là review App Store/Play, group, comment, phỏng vấn nhanh, hoặc nguồn public khác.

| Quote / review / observation | Nguồn | User là ai? | Pain/failure mode |
|---|---|---|---|
| "App đặt phòng tiện nhưng muốn hỏi giờ mở cửa hồ bơi hay xe điện thì tìm hoài không thấy, chatbot trả lời chung chung." | App Store / Play Store Review | Khách đang lưu trú tại Vinpearl | Thiếu context. Bot không biết user đang ở khách sạn nào để đưa ra câu trả lời phù hợp. |
| "Muốn đặt combo vé máy bay và khách sạn nhưng thao tác rườm rà, hỏi bot thì bot chỉ quăng cái link bắt tự đọc." | Group Review Du Lịch Facebook | Khách du lịch tự túc (FIT) | Bot thiếu khả năng dẫn dắt (step-by-step guidance), quăng link gây đứt gãy UX. |
| "Đặt phòng có trẻ em thì báo lỗi không đủ giường, hỏi bot thì bot không biết tư vấn nên chọn hạng phòng nào." | Group Cộng đồng Vinpearl | Gia đình có con nhỏ | Fallback kém, không gợi ý được option thay thế (VD: thêm extra bed). |

## 4. Competitor / analog evidence

| App / mô hình tham khảo | Họ xử lý task này thế nào? | Pattern học được | Có áp dụng trong 1 ngày không? |
|---|---|---|---|
| **Expedia (Tích hợp ChatGPT)** | Cho phép chat tự nhiên, AI tự nhận diện khách sạn được nhắc đến và lưu vào list "Saved". | Conversational Search -> Save to UI. Không ép user book trực tiếp qua chat. | Có, tách biệt phần Chat và phần UI hiển thị kết quả. |
| **Agoda** | Không dùng bot thuần, dùng Dynamic UI/Form filter cực mạnh và suggest thông minh. | Với tác vụ booking, UI truyền thống hiệu quả hơn Chat. Chat chỉ nên làm Support/Concierge. | Có, thay vì làm bot đặt phòng, hãy làm bot "Tư vấn chọn phòng". |

## 5. Evidence -> Insight

```text
Evidence nổi bật nhất:
User thường chật vật khi yêu cầu booking phức tạp (có trẻ em) qua chat, và cực kỳ thiếu thông tin tiện ích nội khu khi đang lưu trú. Chatbot hiện tại chỉ biết "quăng link".

Insight:
User không chỉ gặp [vấn đề về thao tác đặt phòng].
Thật ra họ cần [một người quản gia (concierge) hiểu ngữ cảnh: biết họ đi mấy người để tư vấn phòng, biết họ đang ở khu nào để báo giờ xe điện/hồ bơi].

Opportunity:
AI có thể giúp bằng cách [tự động trích xuất yêu cầu (số người, địa điểm) để fill sẵn vào form tìm kiếm (Augment booking)] và [trả lời Q&A dựa trên context booking hiện tại của khách (Automate Concierge)].
```

## 6. Evidence đổi SPEC như thế nào?

- [ ] Đổi user chính.
- [ ] Đổi pain statement.
- [x] Đổi build slice.
- [x] Đổi Auto/Aug decision.
- [ ] Đổi 4 paths.
- [x] Đổi failure mode.
- [ ] Đổi owner/test plan.

Ghi rõ 1-2 thay đổi quan trọng:

```text
Trước evidence, nhóm định: 
Xây dựng một con Bot có thể tự động hoàn tất việc đặt phòng (Booking) từ A-Z hoàn toàn qua giao diện chat (Automate).

Sau evidence, nhóm đổi thành: 
Chuyển Build slice sang AI Concierge (Trợ lý lưu trú) & AI Search Augmentation. Khi khách tìm phòng, bot chỉ bóc tách Entity (ngày, người) và đẩy ra UI Native (Augment). Khi khách lưu trú, bot lấy context booking để trả lời các câu hỏi tiện ích.

Lý do: 
Việc thanh toán và chọn phòng qua Chat UX rất tệ và dễ lỗi (Failure cao). Việc kết hợp giữa Chat (để lấy intent) và Native UI (để hiển thị và thao tác) như Expedia làm sẽ mang lại trải nghiệm mượt mà và khả thi hơn.
```
