# REFLECTION.md

## 1. Overview

Hệ thống xây dựng một **Multi-Memory Agent (LangGraph)** gồm:

- Short-term memory (chat history)
- User profile memory
- Episodic memory (JSON)
- Semantic memory (knowledge)

Có **memory router** để kết hợp và inject vào prompt.

---

## 2. Đánh giá từng memory

### Short-term
- Giữ context hội thoại
- Giúp xử lý follow-up (he, it)
- Hạn chế: chỉ trim theo turn, chưa theo token

---

### User Profile
- Lưu thông tin cá nhân (tên, sở thích, dị ứng)
- Có **conflict handling (overwrite)**
- Hạn chế: rule-based, chưa robust

---

### Episodic
- Lưu interaction quan trọng (Q&A)
- Hỗ trợ recall task trước đó
- Hạn chế: JSON không scale, chưa phân loại

---

### Semantic
- Lưu knowledge dài hạn
- Retrieve bằng keyword
- Hạn chế: không có embedding → kém chính xác hơn vector search

---

## 3. Memory Router

- Gom: profile + episodic + semantic + history
- Inject vào prompt rõ ràng
- Hạn chế: chưa có weighting / intent-based routing

---

## 4. So sánh với No-Memory

| Tiêu chí | No-memory | With-memory |
|----------|----------|------------|
| Multi-turn | ❌ | ✅ |
| Personalization | ❌ | ✅ |
| Recall | ❌ | ✅ |
| Knowledge reuse | ❌ | ✅ |

→ Memory agent tốt hơn rõ rệt

---

## 5. Privacy & Safety

### Rủi ro
- User profile chứa PII → dễ leak

### Thiếu sót
- Chưa có delete API
- Chưa có TTL

### Cải thiện
- Xóa dữ liệu theo user
- Mã hóa dữ liệu
- Giới hạn thời gian lưu

---

## 6. Hạn chế hệ thống

- Semantic search yếu (keyword-based)
- Không có token-level control
- JSON storage không scale
- Có nguy cơ memory leak giữa test

---

## 7. Hướng cải thiện

- Dùng vector DB (FAISS/Chroma)
- Thêm category cho episodic
- Router theo intent
- Dùng Redis thay JSON


