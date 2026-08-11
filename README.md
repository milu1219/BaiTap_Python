# Password Manager

Ứng dụng quản lý mật khẩu được xây dựng bằng **Python** trong khuôn khổ bài tập nhóm.

Mục tiêu của project là áp dụng kiến thức Python vào một ứng dụng thực tế, đồng thời thực hành làm việc nhóm, quản lý mã nguồn bằng Git/GitHub, kiểm thử và tổ chức một project phần mềm.

## Mục tiêu

* Quản lý thông tin tài khoản và mật khẩu.
* Tạo mật khẩu ngẫu nhiên.
* Bảo vệ dữ liệu mật khẩu bằng các cơ chế mã hóa phù hợp.
* Lưu trữ dữ liệu bằng cơ sở dữ liệu.
* Kiểm tra một số vấn đề liên quan đến độ an toàn của mật khẩu.
* Thực hành quy trình phát triển phần mềm theo nhóm.

## Tính năng dự kiến

* [ ] Tạo mật khẩu ngẫu nhiên
* [ ] Thêm và quản lý tài khoản
* [ ] Chỉnh sửa và xóa thông tin
* [ ] Lưu trữ dữ liệu
* [ ] Mã hóa dữ liệu nhạy cảm
* [ ] Kiểm tra độ mạnh của mật khẩu
* [ ] Kiểm tra mật khẩu có liên quan đến các vụ rò rỉ dữ liệu
* [ ] Giao diện người dùng
* [ ] Kiểm thử các chức năng chính

> Danh sách trên được cập nhật trong quá trình phát triển project.

## Công nghệ sử dụng

* **Python**
* **SQLite**
* **Git**
* **GitHub**

Các thư viện Python sẽ được quản lý trong `requirements.txt`.

## Cấu trúc project

```text
Password-Manager/
│
├── README.md
├── MASTER_PROJECT_PLAN.md
├── requirements.txt
├── .gitignore
│
├── src/
│   └── ...
│
└── tests/
    └── ...
```

### Một số file và thư mục chính

* `README.md` — Giới thiệu và hướng dẫn sử dụng project.
* `MASTER_PROJECT_PLAN.md` — Kế hoạch phát triển chính của project, bao gồm nhiệm vụ theo từng tuần.
* `requirements.txt` — Danh sách thư viện Python cần thiết.
* `src/` — Source code của ứng dụng.
* `tests/` — Các bài kiểm thử.
* `.gitignore` — Các file và thư mục không đưa lên GitHub.

## Cài đặt

Clone repository:

```bash
git clone <repository-url>
cd Password-Manager
```

Tạo virtual environment:

```bash
python -m venv .venv
```

Kích hoạt virtual environment trên Windows:

```bash
.venv\Scripts\activate
```

Cài đặt các thư viện:

```bash
pip install -r requirements.txt
```

## Chạy project

Project hiện đang trong quá trình phát triển.

Cách chạy chính thức sẽ được cập nhật khi cấu trúc ứng dụng và entry point hoàn thiện.

## Kiểm thử

Các bài kiểm thử được đặt trong thư mục `tests/`.

Khi hệ thống test được hoàn thiện, có thể chạy bằng:

```bash
pytest
```

## Tiến độ

Project được phát triển theo kế hoạch trong:

`MASTER_PROJECT_PLAN.md`

Kế hoạch này là nguồn chính để theo dõi:

* Nhiệm vụ từng tuần
* Phân công thành viên
* Milestone
* Deliverable
* Tiến độ phát triển

## Thành viên

Project được thực hiện bởi nhóm sinh viên.

| Thành viên | Vai trò     |
| ---------- | ----------- |
| Member 1   | Development |
| Member 2   | Development |
| Member 3   | Development |

Chi tiết phân công được quản lý trong `MASTER_PROJECT_PLAN.md`.

## Lưu ý

Đây là **project phục vụ mục đích học tập**, không nên sử dụng để lưu trữ các mật khẩu quan trọng trong môi trường thực tế.

Project vẫn đang được phát triển và các tính năng có thể thay đổi trong quá trình thực hiện.
