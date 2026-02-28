**Vietnamese**
# Hệ Thống Quản Lý Công Việc Nội Bộ - Đức Trí School

## Mục Tiêu
Xây dựng hệ thống quản lý công việc nội bộ giúp nhân viên trường Đức Trí:
- **Tạo, phê duyệt, và quản lý** công việc nội bộ.
- **Theo dõi tiến độ** công việc và hoạt động của nhân viên.
- **Quản lý người dùng** và phân quyền theo vai trò.

---

## Đối Tượng Sử Dụng
Hệ thống sử dụng mô hình **RBAC (Role-Based Access Control)** với 3 cấp vai trò:

### 1. **Super Admin:**
- Xem tất cả tasks trong hệ thống.
- Tạo task và giao cho bất kỳ user nào.
- Duyệt task, từ chối task, và thay đổi trạng thái của bất kỳ task nào.
- **Quản lý người dùng** (User CRUD) và **Email Whitelist**.
- Xem **Audit Logs** toàn hệ thống.
- Nhận thông báo khi được chọn làm giám sát.
- Quản lý vai trò và có backup account (account dự phòng hệ thống).

### 2. **Admin:**
- Tạo task request và đợi phê duyệt từ Super Admin.
- Xem các tasks liên quan (task được giao, phối hợp, giám sát).
- Tạo task và giao cho Member.
- **Thay đổi trạng thái** các task do mình tạo hoặc phụ trách.
- Xem danh sách và hoạt động của người dùng.
- *Lưu ý: Không được duyệt task, không quản lý Email Whitelist, không quản lý vai trò người dùng.*

### 3. **Member:**
- Tạo task request và đợi phê duyệt từ Super Admin.
- Chọn người giám sát (Admin / Super Admin) và người phối hợp (Member khác).
- **Cập nhật trạng thái** task mình phụ trách.
- *Lưu ý: Không thay đổi người được giao task, không duyệt task, không xem task ngoài phạm vi, không quản lý người dùng.*

---

## Middleware (Backend) Bảo Vệ Route
- `protect`: Yêu cầu đăng nhập (áp dụng tất cả user).
- `isAdmin`: Chỉ **Admin** và **Super Admin**.
- `isSuperAdmin`: Chỉ **Super Admin**.

---

## Phạm Vi & Chức Năng Chính (Features)

### Xác Thực & Phân Quyền
- **Đăng nhập bằng Google OAuth:** Chỉ email nội bộ trường mới được phép đăng nhập.
- **Email Whitelist:** Super Admin quản lý danh sách email hợp lệ.
- **Tự động đăng xuất:** Sau 15 phút không hoạt động.
- **Quản lý phiên làm việc:** Theo dõi IP, thiết bị, thời gian đăng nhập.

### Quản Lý Công Việc (Task Management)
- **Tạo & Giao việc:** Member/Admin tạo request, Super Admin phê duyệt và có thể gán cho nhiều nhân viên.
- **Workflow:** Chờ phê duyệt -> Đã phê duyệt -> Công khai -> Đang thực hiện -> Hoàn thành.
- **Mức độ ưu tiên:** Low, Medium, High.
- **Hạn chót (Deadline):** Thiết lập thời hạn cho công việc.
- **Đính kèm tài liệu:** Lưu trữ links tài liệu liên quan (JSONB).
- **Lọc và tìm kiếm:** Theo trạng thái, mức độ, người được giao.

### Chia Sẻ Tài Liệu (Shared Documents)
- **Upload & Chia sẻ:** Chia sẻ link tài liệu cho đối tượng cụ thể.
- **Quản lý tài liệu:** Xem, sửa, xóa tài liệu đã chia sẻ.

### Quản Lý Người Dùng (Super Admin Only)
- **CRUD Người dùng:** Xem chi tiết, thay đổi vai trò, xóa tài khoản.
- **Theo dõi hoạt động:** Xem IP, thiết bị, thời gian, và trạng thái Online/Offline.

### Dashboard và Thống Kê
- **Phân quyền hiển thị:** Member (task cá nhân), Admin (tất cả task), Super Admin (tất cả task + users).
- **Thống kê chi tiết:** Tổng task, chờ duyệt, đang thực hiện, hoàn thành, quá hạn.
- **Hệ thống thông báo:** Tích hợp trực tiếp trên giao diện.
- **Audit Logs:** Ghi lại mọi hành động trong hệ thống.

### Giao Diện Người Dùng (UI/UX)
- **Dark/Light Mode:** Hỗ trợ chuyển đổi theme.
- **Responsive Design:** Tối ưu cho cả Mobile và Desktop.
- **Quản lý Profile:** Upload Avatar (lưu trên server), Modal Settings chỉnh sửa thông tin cá nhân.

---

## Công Nghệ Sử Dụng (Tech Stack)

### Frontend
- **React 19.2.0:** Framework UI
- **Vite 7.2.4:** Build tool & Dev server
- **React Router DOM 7.12.0:** Điều hướng
- **Axios 1.13.2:** Gọi API
- **@react-oauth/google:** Đăng nhập Google
- **CSS3:** Styling với CSS variables cho theme

### Backend
- **Node.js 18:** Runtime environment
- **Express.js 5.2.1:** Web framework
- **PostgreSQL 15:** Database (Client: pg 8.16.3)
- **JWT:** Xác thực token
- **Google Auth Library:** Xác thực Google OAuth
- **Multer 1.4.5:** Upload file

### Deployment
- **Docker & Docker Compose:** Containerization
- **Docker Swarm:** Orchestration (Production)
- **Proxmox:** Virtualization platform
- **Nginx & Nginx Proxy Manager:** Web server, Reverse proxy, SSL (Let's Encrypt)
- **Portainer:** Quản lý Docker Swarm

**English**
# Internal Task Management System - Duc Tri School

## Objective
Build an internal task management system to help Duc Tri school staff:
- **Create, approve, and manage** internal tasks.
- **Track the progress** of tasks and employee activities.
- **Manage users** and role-based permissions.

---

## Target Users
The system utilizes an **RBAC (Role-Based Access Control)** model with 3 role levels:

### 1. **Super Admin:**
- View all tasks within the system.
- Create tasks and assign them to any user.
- Approve, reject, and change the status of any task.
- **User Management** (User CRUD) and **Email Whitelist** management.
- View system-wide **Audit Logs**.
- Receive notifications when assigned as a supervisor.
- Manage roles and maintain a backup account (system fallback account).

### 2. **Admin:**
- Create task requests and await approval from the Super Admin.
- View related tasks (assigned, coordinated, supervised).
- Create tasks and assign them to Members.
- **Change the status** of tasks they created or are responsible for.
- View the user list and user activities.
- *Note: Cannot approve tasks, manage the Email Whitelist, or manage user roles.*

### 3. **Member:**
- Create task requests and await approval from the Super Admin.
- Select a supervisor (Admin / Super Admin) and coordinator (other Members).
- **Update the status** of tasks they are responsible for.
- *Note: Cannot change task assignees, approve tasks, view tasks outside their scope, or manage users.*

---

## Route Protection (Backend Middleware)
- `protect`: Requires authentication (applies to all users).
- `isAdmin`: Accessible only by **Admin** and **Super Admin**.
- `isSuperAdmin`: Accessible only by **Super Admin**.

---

## Scope & Key Features

### Authentication & Authorization
- **Google OAuth Login:** Strictly limited to internal school emails.
- **Email Whitelist:** Super Admin manages the list of authorized emails.
- **Auto-logout:** Automatically logs users out after 15 minutes of inactivity.
- **Session Management:** Tracks IP address, device, and login time.

### Task Management
- **Create & Assign:** Members/Admins create requests; Super Admin approves and can assign tasks to multiple employees.
- **Workflow:** Pending Approval -> Approved -> Public -> In Progress -> Completed.
- **Priority Levels:** Low, Medium, High.
- **Deadlines:** Set time limits for task completion.
- **Attachments:** Store links to related documents (JSONB).
- **Filter & Search:** Filter by status, priority, and assignee.

### Shared Documents
- **Upload & Share:** Share document links with specific users.
- **Document Management:** View, edit, and delete shared documents.

### User Management (Super Admin Only)
- **User CRUD:** View detailed profiles, change roles, and delete accounts.
- **Activity Tracking:** Monitor IP, device, timestamps, and Online/Offline status.

### Dashboard & Statistics
- **Role-based Display:** Members (personal tasks only), Admins (all tasks), Super Admins (all tasks + users).
- **Detailed Statistics:** Total tasks, pending approval, in progress, completed, and overdue.
- **Notification System:** Real-time notifications integrated directly into the UI.
- **Audit Logs:** Record every action taken within the system.

### User Interface (UI/UX)
- **Dark/Light Mode:** Seamless theme switching.
- **Responsive Design:** Fully optimized for both Mobile and Desktop.
- **Profile Management:** Upload Avatars (stored on the server) and update personal information via a Settings Modal.

---

## Tech Stack

### Frontend
- **React 19.2.0:** UI Framework
- **Vite 7.2.4:** Build tool & Dev server
- **React Router DOM 7.12.0:** Routing
- **Axios 1.13.2:** API Client
- **@react-oauth/google:** Google Authentication integration
- **CSS3:** Styling with CSS variables for theming

### Backend
- **Node.js 18:** Runtime environment
- **Express.js 5.2.1:** Web framework
- **PostgreSQL 15:** Database (Client: pg 8.16.3)
- **JWT:** Token-based authentication
- **Google Auth Library:** Google OAuth verification
- **Multer 1.4.5:** File uploading

### Deployment
- **Docker & Docker Compose:** Containerization
- **Docker Swarm:** Orchestration (Production)
- **Proxmox:** Virtualization platform
- **Nginx & Nginx Proxy Manager:** Web server, Reverse proxy, SSL (Let's Encrypt)
- **Portainer:** Docker Swarm management
- 
**Login Page:**
<img width="1917" height="957" alt="image1" src="https://github.com/user-attachments/assets/c11aa002-a929-4524-95eb-ec1614bfb7de" />

**Admin(Backup Super Admin):**
<img width="1916" height="958" alt="image3" src="https://github.com/user-attachments/assets/3406e06f-fcac-403f-9df0-6c1a3f8ebe6a" />

**Home Page:**
<img width="1894" height="926" alt="image5" src="https://github.com/user-attachments/assets/83ecaade-f4c9-41ba-b252-8a65dcae20c6" />

**Audit Logs:**
<img width="1916" height="956" alt="image8" src="https://github.com/user-attachments/assets/76cb8985-f9c9-42d2-96c0-9a93ac843be3" />
