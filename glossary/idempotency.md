---
title: "Idempotency"
defaultLocale: vi
---

# vi

Một thao tác có tính idempotent khi gọi lại cùng yêu cầu nhiều lần vẫn tạo ra tác động dự kiến giống như gọi một lần. Khái niệm này đặc biệt quan trọng khi hệ thống phải thử lại sau lỗi mạng mà không muốn vô tình tạo hai đơn hàng, hai khoản thanh toán hoặc hai lời mời.

Tìm hiểu thêm: [RFC 9110 - Idempotent Methods](https://www.rfc-editor.org/rfc/rfc9110.html#section-9.2.2).

# en

An operation is idempotent when repeating the same request has the same intended effect as performing it once. This matters when a system retries after a network failure without wanting to create duplicate records or actions.

Learn more: [RFC 9110 - Idempotent Methods](https://www.rfc-editor.org/rfc/rfc9110.html#section-9.2.2).
