---
bump: patch
title: Add opt-in compact subscription context and stricter memory retrieval
---
## vi
### Cải thiện
- Thêm chế độ ngữ cảnh gọn cho Codex subscription, giữ hồ sơ và chỉ dẫn project, phiên. Quản trị viên có thể bật riêng mà không đổi model.
- Thêm bộ lọc trí nhớ theo chủ đề, cho phép không nạp đoạn nhớ không liên quan. Mặc định vẫn giữ chế độ cũ để triển khai có kiểm soát.

- Sửa điều kiện xoay mạch Codex để dùng số token lần gọi gần nhất, tách khỏi tổng cả lượt; không lấy số tổng cũ làm ngữ cảnh.

## en
### Improved
- Add opt-in compact context for Codex subscriptions while retaining profile, project and session instructions without changing the selected model.
- Add opt-in topic filtering for memory retrieval, including empty results when no fact is relevant. Existing defaults remain unchanged for controlled rollout.
- Use the latest measured Codex request input for thread rotation instead of the turn sum, without migrating old sums into request context.
