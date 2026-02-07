# 📱 Hướng Dẫn Quản Trị - Phone Registration System

## Tổng Quan

Hệ thống đăng ký số điện thoại cho phép quản lý quyền truy cập các tính năng nâng cao:

- **Phân tích tên đề tài** (AI Analysis)
- **Thẩm định toàn bộ SKKN** (Full Appraisal)

---

## 🔐 Đăng Nhập Admin

| Thông tin | Giá trị |
|-----------|---------|
| **Username** | `admin` |
| **Password** | `123` |

> ⚠️ **Bảo mật:** Thay đổi credentials trong `utils/authUtils.ts` (dòng 12-15) trước khi deploy production.

---

## 📊 Flow Hoạt Động

```
Người dùng nhập SĐT → Chờ duyệt → Admin kích hoạt → Dùng tính năng
```

### Trạng thái SĐT

1. **Chờ duyệt** (Pending) - Vừa đăng ký, chưa được kích hoạt
2. **Đã kích hoạt** (Activated) - Có thể sử dụng tính năng nâng cao

---

## 🛠️ Hướng Dẫn Admin

### Vào Trang Quản Trị

1. Click **"Đăng nhập Admin"** ở header
2. Nhập username: `admin`, password: `123`
3. Click **"Quản lý người dùng"** để mở Admin Dashboard

### Kích Hoạt SĐT

1. Mở **Admin Dashboard**
2. Xem danh sách **"Yêu cầu chờ duyệt"**
3. Click nút **"Kích hoạt"** bên cạnh SĐT cần duyệt
4. SĐT sẽ chuyển sang danh sách **"Đã kích hoạt"**

### Hủy Kích Hoạt

1. Trong mục **"Người dùng đã kích hoạt"**
2. Click **"Hủy kích hoạt"** để thu hồi quyền truy cập

---

## 💾 Dữ Liệu Lưu Trữ

Dữ liệu lưu trong `localStorage` của trình duyệt:

| Key | Mô tả |
|-----|-------|
| `skkn_admin_logged_in` | Trạng thái đăng nhập admin |
| `skkn_user_phone` | SĐT của user hiện tại |
| `skkn_pending_phones` | Mảng JSON các SĐT chờ duyệt |
| `skkn_activated_phones` | Mảng JSON các SĐT đã kích hoạt |

### Xem Dữ Liệu (DevTools)

```javascript
// Mở Console (F12) và chạy:
JSON.parse(localStorage.getItem('skkn_pending_phones'))
JSON.parse(localStorage.getItem('skkn_activated_phones'))
```

### Xóa Toàn Bộ Dữ Liệu

```javascript
localStorage.removeItem('skkn_pending_phones');
localStorage.removeItem('skkn_activated_phones');
localStorage.removeItem('skkn_user_phone');
```

---

## 📁 Cấu Trúc Files

| File | Chức năng |
|------|-----------|
| `components/PhoneRegisterModal.tsx` | Modal đăng ký SĐT cho user |
| `components/AdminLoginModal.tsx` | Modal đăng nhập admin |
| `components/AdminDashboardModal.tsx` | Dashboard quản lý SĐT |
| `utils/authUtils.ts` | Logic xử lý xác thực |

---

## ⚙️ Các Hàm API (authUtils.ts)

| Hàm | Mô tả |
|-----|-------|
| `validateAdminCredentials(user, pass)` | Kiểm tra đăng nhập admin |
| `isAdmin()` | Kiểm tra đang là admin không |
| `loginAdmin()` / `logoutAdmin()` | Đăng nhập/đăng xuất |
| `registerPhone(phone)` | Đăng ký SĐT mới (thêm vào pending) |
| `activatePhone(phone)` | Kích hoạt SĐT (chuyển pending → activated) |
| `deactivatePhone(phone)` | Hủy kích hoạt |
| `canAccessFeature()` | Kiểm tra quyền truy cập tính năng |
| `getPendingPhones()` | Lấy danh sách pending |
| `getActivatedPhones()` | Lấy danh sách activated |

---

## 🔧 Tùy Chỉnh

### Thay Đổi Mật Khẩu Admin

Edit file `utils/authUtils.ts`:

```typescript
// Dòng 12-15
const ADMIN_CREDENTIALS = {
  username: 'admin',
  password: 'matkhaumoi123',  // Đổi ở đây
};
```

### Thay Đổi Format SĐT

Edit file `utils/authUtils.ts`:

```typescript
// Dòng 105
const phoneRegex = /^0\d{9,10}$/;  // VN: 10-11 số, bắt đầu bằng 0
```

---

## ⚠️ Lưu Ý Quan Trọng

1. **Dữ liệu theo trình duyệt:** Mỗi trình duyệt có localStorage riêng
2. **Không đồng bộ:** Admin và user phải dùng cùng trình duyệt/máy
3. **Production:** Nên chuyển sang database (Firebase, Supabase) để đồng bộ giữa các thiết bị
