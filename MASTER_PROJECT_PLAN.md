# MASTER PROJECT PLAN — PASSWORD MANAGER
> Môn: Nhập môn Python | Team: 3 sinh viên beginner | Timeline: W2–W9

---

## 1. CRITICAL AUDIT

### 1.1 Course Fit
✅ **Phù hợp.** Project bao gồm: OOP, file I/O, SQLite, Tkinter, testing — đúng nội dung Nhập môn Python. Không có gì vượt khỏi phạm vi môn học nếu scope được kiểm soát tốt.

### 1.2 Difficulty
⚠️ **Một số chỗ quá khó cho beginner:**
- PBKDF2 + Fernet: khó hiểu conceptually nếu không giải thích đúng → cần simplify quy trình học.
- `calculate_entropy()` trong W3: không cần thiết, over-engineering.
- `check_pwned()` dùng local list: OK nếu chỉ là list text tĩnh, nhưng tên hàm dễ nhầm với HaveIBeenPwned API → đổi tên.
- `export_to_csv` / `import_from_csv` trong W3: quá sớm, sẽ distract khỏi Core MVP.

### 1.3 Scope Problems
- **W3 Member 2** làm Export/Import CSV: đây là SHOULD HAVE, không phải MUST HAVE. Lúc này core chưa chạy mà đã làm export là sai thứ tự.
- **W3 Member 1** làm `check_pwned` và `calculate_entropy`: entropy là nice-to-have. Pwned check (local) là OK nhưng đừng gọi là "check_pwned" — dễ hiểu nhầm.
- **W4** dồn toàn bộ integration vào 1 tuần: rủi ro cao.

### 1.4 Team Balance Problems
- **Member 3** trong W4 bị đẩy vào vai trò "người ghép code": nối login, nối Treeview, nối CRUD buttons — trong khi M1 và M2 chỉ "kiểm tra đồng bộ". ❌
- **Member 2** trong W3 làm Export/Import thay vì củng cố database → workload không đúng ưu tiên.
- **Không có interface contract**: 3 người code riêng W2–W3, W4 mới gặp nhau → rủi ro mismatch cao.

### 1.5 Dependency & Parallelism
❌ Không có mock/stub/contract → W2–W3 dễ bị block khi cần dữ liệu từ module khác.  
✅ Fix: Định nghĩa Interface Contract cuối W2, dùng mock data để UI và DB có thể test độc lập.

### 1.6 Technical Risk Assessment

| Công nghệ | Đánh giá | Quyết định |
|-----------|----------|------------|
| `cryptography` (Fernet, PBKDF2) | Cần thiết, có tài liệu tốt | **KEEP** — học có hướng dẫn |
| `sqlite3` | Stdlib, phù hợp beginner | **KEEP** |
| `tkinter` / `ttk` | Stdlib, đủ dùng | **KEEP** |
| `secrets` | Stdlib, đơn giản | **KEEP** |
| `pytest` | Đơn giản, recommended | **KEEP** |
| `calculate_entropy()` | Nice-to-have, phức tạp không cần thiết | **SIMPLIFY** → chỉ báo Weak/Medium/Strong bằng rule đơn giản |
| `check_pwned()` | Local list OK, tên gây nhầm | **SIMPLIFY** → đổi thành `check_common_password()` |
| CSV Export/Import | Hữu ích nhưng không phải core | **OPTIONAL** — làm sau W6 nếu kịp |
| HaveIBeenPwned API | Network, auth — quá phức tạp | **REMOVE** |
| Auto-lock | Stateful timer — phức tạp | **REMOVE** |
| Password Audit dashboard | Quá nhiều UI logic | **REMOVE** |
| Breach API | External dependency | **REMOVE** |

---

## 2. PROBLEMS FOUND

1. **Không có Interface Contract** — 3 người làm riêng W2–W3, W4 integration sẽ bị mismatch.
2. **Member 3 gánh Integration** — W4 thiết kế cho M3 nối tất cả mọi thứ.
3. **Export/Import quá sớm** — W3 M2 làm CSV thay vì consolidate database.
4. **`calculate_entropy` không cần thiết** — beginner không cần hiểu entropy formula.
5. **Testing không có timeline rõ** — chỉ nói "unit test" nhưng không specify khi nào.
6. **W4 Integration = Big Bang** — lần đầu 3 module gặp nhau = rủi ro cao.
7. **Daily task quá gộp** — "Thứ 2 - Thứ 3" là 2 ngày gộp, không phải daily task.

---

## 3. CHANGES MADE

1. **Thêm Interface Contract session cuối W2** (toàn nhóm cùng làm, ~1–2 giờ).
2. **Redistribute W4**: Integration là trách nhiệm chung, không phải của M3.
3. **Di chuyển CSV Export sang W6** (SHOULD HAVE, sau MVP).
4. **Thay `calculate_entropy` bằng `check_password_strength()`** — rule-based đơn giản.
5. **Thêm Unit Test vào W2** cho tất cả 3 member.
6. **Thêm Checkpoint nhỏ W3** — mock integration test giữa M1 và M2.
7. **Tách daily task thành từng ngày**.
8. **Bỏ Auto-lock, Breach API, Audit Dashboard** khỏi scope.

---

## 4. FINAL SCOPE

### MUST HAVE (Core MVP — không được cắt)
- [x] Master Password đăng nhập
- [x] Lưu/đọc password entries vào SQLite (encrypted)
- [x] Xem danh sách accounts
- [x] Thêm account mới
- [x] Sửa account
- [x] Xóa account
- [x] Password Generator (length + charset)
- [x] Show/Hide password trên UI
- [x] Copy to Clipboard
- [x] Search/Filter theo tên

### SHOULD HAVE (Làm sau MVP, trước W8)
- [ ] Password strength indicator (Weak/Medium/Strong)
- [ ] Check common passwords (local list)
- [ ] Export to CSV
- [ ] Category filter

### COULD HAVE (Chỉ làm nếu W8 vẫn còn thời gian)
- [ ] Import from CSV
- [ ] Màu sắc/theme UI
- [ ] Sort by column

### REMOVE (Không làm)
- ❌ HaveIBeenPwned API (network dependency)
- ❌ Auto-lock (timer complexity)
- ❌ Password Audit dashboard
- ❌ `calculate_entropy()` (formula không cần thiết)

---

## 5. FINAL TEAM ALLOCATION

| Member | Primary | Secondary | Testing | Integration |
|--------|---------|-----------|---------|-------------|
| M1 | `security.py` + `generator.py` | Hỗ trợ M2 kết nối crypto với DB | Test unit security functions | Tham gia W4 integration, viết crypto-DB bridge |
| M2 | `database.py` | `search` + `export` (W6) | Test unit CRUD functions | Tham gia W4 integration, verify data flow |
| M3 | `ui/` (Tkinter windows) | `app.py` controller layer | Test UI flows (manual + basic) | Tham gia W4 integration, nối UI events |

### Giải thích workload cân bằng:
- M1: security logic phức tạp về concept nhưng code không nhiều dòng → bù bằng generator.
- M2: database CRUD là phần nhiều code nhất → không thêm feature phụ ở đầu.
- M3: UI nhiều file nhưng Tkinter là widget-based → không bị gánh integration một mình.
- **Integration W4**: cả 3 ngồi cùng nhau hoặc pair programming theo cặp M1-M2, M2-M3, M1-M3.

---

## 6. FINAL ARCHITECTURE

### Security Flow (quan trọng — phải hiểu rõ)

```
User nhập Master Password
        ↓
PBKDF2HMAC(password, salt) → Derived Key (32 bytes)
        ↓
Fernet(derived_key) → cipher object
        ↓
encrypt: cipher.encrypt(plaintext.encode()) → ciphertext (bytes)
decrypt: cipher.decrypt(ciphertext) → plaintext
```

**Câu hỏi quan trọng — trả lời rõ:**

| Câu hỏi | Trả lời |
|---------|---------|
| Master Password có được lưu không? | **KHÔNG** — không bao giờ lưu raw password |
| Salt lưu ở đâu? | Lưu trong SQLite, bảng `config`, dưới dạng hex/base64 |
| Derived key lưu ở đâu? | **Chỉ trong RAM** — tạo mỗi lần login, xóa khi thoát |
| Password entries lưu dạng gì? | **Ciphertext** (bytes) trong SQLite |
| Giải mã khi nào? | Chỉ khi user yêu cầu xem — decrypt on demand |
| Tuyệt đối không commit lên GitHub | `passwords.db`, bất kỳ file chứa real data |

### Module Structure

```
security.py       ← M1: PBKDF2 key derivation, Fernet encrypt/decrypt
generator.py      ← M1: password generation, strength check
database.py       ← M2: SQLite CRUD, search
ui/
  login_window.py ← M3: Master Password screen
  main_window.py  ← M3: Main app frame + Treeview
  dialogs.py      ← M3: Add/Edit/Generator dialogs
app.py            ← M3 (+ team): controller/orchestrator
```

### Interface Contract (định nghĩa W2, dùng từ W3)

```python
# security.py exports:
def derive_key(master_password: str, salt: bytes) -> bytes: ...
def encrypt(plaintext: str, key: bytes) -> bytes: ...
def decrypt(ciphertext: bytes, key: bytes) -> str: ...
def generate_salt() -> bytes: ...

# database.py exports:
def create_tables() -> None: ...
def add_account(service: str, username: str, password_enc: bytes, url: str, category: str) -> int: ...
def get_all_accounts() -> list[dict]: ...
def get_account(account_id: int) -> dict: ...
def update_account(account_id: int, **kwargs) -> None: ...
def delete_account(account_id: int) -> None: ...
def search_accounts(keyword: str) -> list[dict]: ...

# generator.py exports:
def generate_password(length: int, use_upper: bool, use_digits: bool, use_symbols: bool) -> str: ...
def check_password_strength(password: str) -> str: ...  # returns "Weak"/"Medium"/"Strong"
def check_common_password(password: str, wordlist: list) -> bool: ...
```

---

## 7. FINAL W2–W9 ROADMAP

### W2 — Foundation + Interface Contract
**Goal:** Mỗi member viết module skeleton + unit test đầu tiên. Cả nhóm đồng thuận Interface Contract.
- M1: `security.py` với PBKDF2 + Fernet
- M2: `database.py` với `create_tables()`, `add_account()`, `get_all_accounts()`
- M3: Tkinter skeleton — `login_window.py` + `main_window.py` frame tĩnh
- **Cuối tuần:** Interface Contract session (cả nhóm) + smoke test mỗi module

### W3 — Core Logic + Mock Integration
**Goal:** Hoàn thành logic chính, test với mock data, verify module boundaries.
- M1: `generator.py` (generate + strength check), thêm `check_common_password()`
- M2: `update_account()`, `delete_account()`, `search_accounts()`
- M3: `dialogs.py` (Add/Edit form), Generator UI, Show/Hide, Copy Clipboard
- **Giữa tuần:** M1 + M2 chạy integration test nhỏ: encrypt → store → retrieve → decrypt với mock DB

### W4 — First Runnable MVP
**Goal:** App chạy end-to-end. Login → Xem list → Thêm (encrypted) → Lưu → Hiển thị.
- **Cả nhóm** làm integration — mỗi người connect module của mình vào `app.py`
- M1: Kết nối security với DB (encrypt trước khi insert, decrypt sau khi fetch)
- M2: Verify data types nhất quán, test với real encrypted data
- M3: Kết nối UI events với controller functions trong `app.py`
- **Cuối tuần:** Demo internal — app chạy được basic flow

### W5 — Feature Completion
**Goal:** Hoàn thiện tất cả MUST HAVE features.
- M1: Integrate generator vào UI (nút "Generate" trong dialog)
- M2: `search_accounts()` nối với UI search bar
- M3: Delete confirmation dialog, Edit pre-populate form, UI polish cơ bản
- Testing: tất cả unit test phải pass

### W6 — SHOULD HAVE + Stabilization
**Goal:** Thêm SHOULD HAVE nếu MUST HAVE đã ổn. Fix bugs từ W5.
- M1: `check_password_strength()` hiển thị trên UI, `check_common_password()` cảnh báo
- M2: Export to CSV (nếu còn thời gian)
- M3: Category filter trong Treeview, UI cleanup
- Integration test: full flow end-to-end với nhiều accounts

### W7 — Testing & Bug Fix
**Goal:** Viết test có hệ thống, fix bugs được phát hiện.
- M1: Bổ sung unit test security edge cases (sai key, empty input)
- M2: Bổ sung unit test DB (duplicate, empty, special chars)
- M3: Manual test toàn bộ UI flows, document bugs
- Integration test: test các scenario quan trọng
- **Scope Freeze:** Không thêm feature mới sau W7

### W8 — Full Integration & Polish
**Goal:** App hoàn chỉnh, stable, sẵn sàng demo.
- Toàn nhóm: fix bugs từ W7 testing
- Regression test sau mỗi fix
- Cleanup code (comments, docstrings, remove debug prints)
- **Feature Freeze:** Không sửa logic sau W8 Day 5
- Chuẩn bị demo script

### W9 — Final Stabilization & Submission
**Goal:** Nộp bài.
- **Code Freeze:** Day 1 của W9 — không thêm code mới
- Viết/hoàn thiện ROADMAP.md, REQUIREMENTS.md, ARCHITECTURE.md
- README.md hướng dẫn cài đặt và chạy
- Final demo run
- Nộp bài

---

## 8. COMPLETE DAILY PLAN W2–W9

> **Quy ước:**
> - `[L]` = Learning (đọc tài liệu, xem ví dụ)
> - `[C]` = Coding (viết code thực tế)
> - `[T]` = Testing (chạy test, verify)
> - `[O]` = Output (file/function cụ thể phải có)
> - `[DoD]` = Definition of Done

---

### TUẦN 2 — Foundation + Interface Contract

#### THỨ 2

**M1**
- `[L]` Đọc docs `cryptography` library: PBKDF2HMAC, Fernet — tập trung vào ví dụ code
- `[C]` Tạo file `src/security.py`. Viết function `generate_salt()` trả về `os.urandom(16)`
- `[T]` Chạy Python REPL, gọi `generate_salt()`, in kết quả, verify là bytes 16 ký tự
- `[O]` `security.py` có `generate_salt()` hoạt động
- `[DoD]` Gọi `generate_salt()` 3 lần → 3 giá trị khác nhau

**M2**
- `[L]` Đọc Python `sqlite3` docs: `connect()`, `cursor()`, `execute()`, `commit()`
- `[C]` Tạo file `src/database.py`. Viết `create_connection(db_path)` trả về connection object
- `[T]` Chạy script: tạo connection tới `test.db`, verify file `test.db` xuất hiện trên disk
- `[O]` `database.py` có `create_connection()` hoạt động
- `[DoD]` File `test.db` được tạo, không có exception

**M3**
- `[L]` Đọc Tkinter basic: `Tk()`, `Frame`, `Label`, `Entry`, `Button`, `.grid()` layout
- `[C]` Tạo file `src/ui/login_window.py`. Viết class `LoginWindow(tk.Tk)` với 1 Label "Password Manager", 1 Entry (show="*"), 1 Button "Login"
- `[T]` Chạy `login_window.py` trực tiếp, verify cửa sổ hiện ra đúng bố cục
- `[O]` `login_window.py` hiển thị đúng
- `[DoD]` Cửa sổ mở, có đủ 3 widget, Entry ẩn ký tự

---

#### THỨ 3

**M1**
- `[L]` Đọc `PBKDF2HMAC` example trong `cryptography` docs
- `[C]` Viết `derive_key(master_password: str, salt: bytes) -> bytes` trong `security.py` — dùng PBKDF2HMAC với SHA256, iterations=100000, length=32, trả về base64url encoded key
- `[T]` Gọi `derive_key("test123", generate_salt())` → verify trả về bytes, length hợp lệ cho Fernet
- `[O]` `derive_key()` trong `security.py`
- `[DoD]` Không raise exception, key có thể dùng để khởi tạo `Fernet(key)`

**M2**
- `[C]` Viết `create_tables(conn)` trong `database.py`: tạo bảng `accounts` (id INTEGER PRIMARY KEY, service TEXT, username TEXT, password_enc BLOB, url TEXT, category TEXT, created_at TEXT) và bảng `config` (key TEXT, value TEXT)
- `[T]` Chạy `create_tables()`, mở `test.db` bằng DB Browser hoặc Python, verify 2 bảng tồn tại với đúng columns
- `[O]` 2 bảng trong `test.db`
- `[DoD]` `SELECT * FROM accounts` và `SELECT * FROM config` không lỗi

**M3**
- `[C]` Viết function `get_password()` trong `LoginWindow` — đọc giá trị từ Entry, in ra console (chưa nối logic thật)
- `[C]` Thêm `<Return>` key binding để trigger button Login
- `[T]` Chạy app, nhập password, nhấn Enter → verify console in đúng giá trị
- `[O]` `login_window.py` với `get_password()` và key binding
- `[DoD]` Nhấn Enter trong Entry → hàm được gọi

---

#### THỨ 4

**M1**
- `[C]` Viết `encrypt(plaintext: str, key: bytes) -> bytes` trong `security.py` — khởi tạo `Fernet(key)`, gọi `encrypt(plaintext.encode())`, trả về ciphertext bytes
- `[T]` Test: `encrypt("hello", derive_key("pass", salt))` → verify trả về bytes, không phải "hello"
- `[O]` `encrypt()` trong `security.py`
- `[DoD]` Output là bytes, khác hoàn toàn với input plaintext

**M2**
- `[C]` Viết `add_account(conn, service, username, password_enc, url="", category="General") -> int` — INSERT vào bảng `accounts`, trả về `lastrowid`
- `[T]` Gọi `add_account()` với dummy data (password_enc = b"fake_encrypted"), verify bằng `SELECT * FROM accounts`
- `[O]` `add_account()` trong `database.py`
- `[DoD]` Record xuất hiện trong DB, `id` trả về > 0

**M3**
- `[L]` Đọc `ttk.Treeview` docs: columns, headings, insert
- `[C]` Tạo file `src/ui/main_window.py`. Viết class `MainWindow(tk.Toplevel)` với `ttk.Treeview` có 4 columns: ID, Service, Username, Category
- `[T]` Tạo `MainWindow` từ script test, manually insert 2 rows dummy data, verify hiển thị đúng
- `[O]` `main_window.py` với Treeview hiển thị dummy data
- `[DoD]` 2 dummy rows hiển thị trong Treeview

---

#### THỨ 5

**M1**
- `[C]` Viết `decrypt(ciphertext: bytes, key: bytes) -> str` trong `security.py` — `Fernet(key).decrypt(ciphertext).decode()`
- `[T]` Test round-trip: `decrypt(encrypt("secret", key), key) == "secret"` → phải True. Test sai key: `decrypt(ciphertext, wrong_key)` → phải raise `InvalidToken`
- `[O]` `decrypt()` trong `security.py`
- `[DoD]` Round-trip test pass, wrong-key test raise exception đúng

**M2**
- `[C]` Viết `get_all_accounts(conn) -> list[dict]` — SELECT tất cả rows, trả về list of dict với keys: id, service, username, password_enc, url, category
- `[T]` Insert 3 dummy records, gọi `get_all_accounts()`, verify trả về list có 3 phần tử, mỗi phần tử là dict đúng keys
- `[O]` `get_all_accounts()` trong `database.py`
- `[DoD]` List trả về có đúng số records, mỗi dict có đủ keys

**M3**
- `[C]` Viết method `load_accounts(accounts: list[dict])` trong `MainWindow` — clear Treeview, loop qua list, insert từng row
- `[T]` Truyền list dict dummy vào `load_accounts()`, verify Treeview cập nhật đúng
- `[O]` `load_accounts()` trong `main_window.py`
- `[DoD]` Truyền list 3 items → Treeview hiển thị 3 rows

---

#### THỨ 6

**M1**
- `[C]` Viết unit test file `tests/test_security.py` với `pytest`:
  - `test_generate_salt_is_bytes()`
  - `test_derive_key_returns_valid_fernet_key()`
  - `test_encrypt_decrypt_roundtrip()`
  - `test_decrypt_wrong_key_raises()`
- `[T]` Chạy `pytest tests/test_security.py -v`, fix cho tất cả 4 test pass
- `[O]` `tests/test_security.py` với 4 passing tests
- `[DoD]` `pytest` report: 4 passed, 0 failed

**M2**
- `[C]` Viết unit test file `tests/test_database.py`:
  - `test_create_tables_success()`
  - `test_add_account_returns_id()`
  - `test_get_all_accounts_returns_list()`
- Dùng SQLite in-memory (`:memory:`) để test nhanh
- `[T]` Chạy `pytest tests/test_database.py -v`
- `[O]` `tests/test_database.py` với 3 passing tests
- `[DoD]` 3 passed, 0 failed

**M3**
- `[C]` Viết `src/ui/dialogs.py`. Tạo class `AddAccountDialog(tk.Toplevel)` với các Entry: Service, Username, Password, URL, Category. Nút "Save" và "Cancel"
- `[T]` Mở dialog từ script test, verify các field hiển thị, nút Cancel đóng dialog
- `[O]` `dialogs.py` với `AddAccountDialog`
- `[DoD]` Dialog mở được, Cancel đóng được

---

#### THỨ 7

**Cả nhóm — Interface Contract Session (2–3 giờ)**
- Cả 3 ngồi lại (online hoặc offline)
- Review lại function signatures trong `security.py` và `database.py`
- Đảm bảo M3 biết chính xác: hàm nào gọi, tham số là gì, trả về gì
- Viết `docs/INTERFACE_CONTRACT.md` — document các function signatures đã thống nhất
- Tạo GitHub Issues cho W3 (mỗi member tạo issues của mình)
- `[DoD]` File `INTERFACE_CONTRACT.md` được commit lên GitHub, cả 3 đã review

---

#### CHỦ NHẬT — Module Smoke Test + Commit

**M1**
- `[T]` Chạy toàn bộ `pytest tests/test_security.py` — tất cả pass
- `[C]` Thêm docstring cho mỗi function trong `security.py`
- Commit: `"feat: security module - PBKDF2 key derivation, Fernet encrypt/decrypt"`

**M2**
- `[T]` Chạy `pytest tests/test_database.py` — tất cả pass
- `[C]` Thêm docstring cho mỗi function trong `database.py`
- Commit: `"feat: database module - SQLite CRUD foundation"`

**M3**
- `[T]` Chạy `login_window.py` và `main_window.py` thủ công, verify không crash
- `[C]` Cleanup code, xóa debug prints thừa
- Commit: `"feat: UI skeleton - login window, main window, add dialog"`

---

### TUẦN 3 — Core Logic + Mock Integration

#### THỨ 2

**M1**
- `[L]` Đọc Python `secrets` module docs: `secrets.choice()`, `secrets.token_urlsafe()`
- `[C]` Tạo `src/generator.py`. Viết `generate_password(length: int, use_upper: bool, use_digits: bool, use_symbols: bool) -> str` — build charset từ options, dùng `secrets.choice()` trong loop
- `[T]` Gọi `generate_password(12, True, True, True)` 5 lần → verify length=12, có uppercase, digits, symbols
- `[O]` `generator.py` với `generate_password()`
- `[DoD]` 5 passwords khác nhau, đúng length, có đủ charset được chọn

**M2**
- `[C]` Viết `update_account(conn, account_id: int, service=None, username=None, password_enc=None, url=None, category=None) -> None` — chỉ UPDATE các field không None
- `[T]` Insert 1 record, gọi `update_account(id, username="new_user")`, verify bằng SELECT
- `[O]` `update_account()` trong `database.py`
- `[DoD]` Field được update, các field khác không thay đổi

**M3**
- `[C]` Tạo class `EditAccountDialog(tk.Toplevel)` trong `dialogs.py` — giống AddAccountDialog nhưng nhận `account_data: dict` để pre-populate các Entry
- `[T]` Mở EditAccountDialog với dict `{"service": "Google", "username": "test@gmail.com", ...}`, verify các Entry hiển thị đúng giá trị
- `[O]` `EditAccountDialog` trong `dialogs.py`
- `[DoD]` Entry pre-populated đúng với data truyền vào

---

#### THỨ 3

**M1**
- `[C]` Viết `check_password_strength(password: str) -> str` trong `generator.py`:
  - "Weak": độ dài < 8 HOẶC không có uppercase/digit/symbol
  - "Medium": độ dài 8–11 VÀ có ít nhất 2 trong 3 (upper, digit, symbol)
  - "Strong": độ dài >= 12 VÀ có đủ upper, digit, symbol
- `[T]` Test với "abc" → "Weak", "Abc123" → "Medium", "Abc@12345678" → "Strong"
- `[O]` `check_password_strength()` trong `generator.py`
- `[DoD]` 3 test cases đúng kết quả

**M2**
- `[C]` Viết `delete_account(conn, account_id: int) -> None` — DELETE WHERE id = account_id
- `[T]` Insert record, delete, verify `get_all_accounts()` không còn record đó
- `[O]` `delete_account()` trong `database.py`
- `[DoD]` Record bị xóa, list còn lại không có record đó

**M3**
- `[C]` Tạo class `GeneratorDialog(tk.Toplevel)` trong `dialogs.py`: Spinbox cho length (8–32), 3 Checkbutton (Uppercase, Digits, Symbols), Label hiển thị password sinh ra, nút "Generate", nút "Use This Password"
- `[T]` Mở dialog, nhấn Generate → verify Label cập nhật
- `[O]` `GeneratorDialog` hoạt động
- `[DoD]` Generate tạo password mới mỗi lần nhấn

---

#### THỨ 4

**M1**
- `[C]` Tải file `common_passwords.txt` (top 1000 common passwords từ internet). Viết `check_common_password(password: str, wordlist: list[str]) -> bool` — trả về True nếu password nằm trong wordlist
- `[T]` Load wordlist, test `check_common_password("password123", wordlist)` → True; test `check_common_password("X7#mK9@qL2", wordlist)` → False
- `[O]` `check_common_password()` + `common_passwords.txt` trong `data/`
- `[DoD]` 2 test cases đúng

**M2**
- `[C]` Viết `search_accounts(conn, keyword: str) -> list[dict]` — SELECT WHERE service LIKE %keyword% OR username LIKE %keyword%
- `[T]` Insert 3 records với service "Google", "GitHub", "Facebook". Search "git" → verify chỉ trả về GitHub record
- `[O]` `search_accounts()` trong `database.py`
- `[DoD]` Search đúng, case-insensitive nếu được

**M3**
- `[C]` Thêm vào `AddAccountDialog`: nút "Generate Password" mở `GeneratorDialog`, khi user chọn "Use This Password" → populate Entry password trong AddAccountDialog
- `[T]` Test flow: mở AddAccountDialog → click Generate → GeneratorDialog mở → click Use → password điền vào
- `[O]` Integration nhỏ giữa AddAccountDialog và GeneratorDialog
- `[DoD]` Flow trên hoạt động không lỗi

---

#### THỨ 5 — Mock Integration (M1 + M2)

**M1 + M2 (cùng làm)**
- `[C]` Viết script `tests/test_integration_mock.py`:
  - Tạo in-memory DB
  - Tạo salt + derive key từ "master123"
  - Encrypt "my_secret_password" → ciphertext bytes
  - `add_account(conn, "Google", "user@gmail.com", ciphertext, "", "Work")`
  - `accounts = get_all_accounts(conn)`
  - `decrypt(accounts[0]["password_enc"], key)` → verify == "my_secret_password"
- `[T]` Chạy script → verify round-trip thành công
- `[O]` `tests/test_integration_mock.py`
- `[DoD]` Script chạy không lỗi, decrypt ra đúng plaintext

**M3**
- `[C]` Thêm Show/Hide toggle vào Entry password trong `AddAccountDialog` và `EditAccountDialog`: Button với text "👁" toggle `show=""` và `show="*"`
- `[T]` Test toggle: mặc định hidden, click → hiển thị, click lại → ẩn
- `[O]` Show/Hide trong dialogs
- `[DoD]` Toggle hoạt động đúng cả 2 trạng thái

---

#### THỨ 6

**M1**
- `[C]` Viết unit test cho `generator.py` trong `tests/test_generator.py`:
  - `test_generate_password_length()`
  - `test_generate_password_has_digits_when_requested()`
  - `test_check_strength_weak()`
  - `test_check_strength_strong()`
- `[T]` Chạy pytest, fix cho pass
- `[O]` `tests/test_generator.py` với 4 tests passing
- `[DoD]` 4 passed, 0 failed

**M2**
- `[C]` Bổ sung unit test `test_database.py`:
  - `test_update_account_changes_only_specified_fields()`
  - `test_delete_account_removes_record()`
  - `test_search_accounts_returns_matching()`
- `[T]` Chạy pytest
- `[O]` `tests/test_database.py` với 6 tests total
- `[DoD]` 6 passed

**M3**
- `[C]` Thêm "Copy to Clipboard" button trong `main_window.py` — khi user chọn row trong Treeview và nhấn Copy, gọi `root.clipboard_clear()` + `root.clipboard_append(password_plaintext)`
- Lưu ý: password trong Treeview chỉ hiển thị "••••••" — Copy sẽ cần decrypt trước, sẽ hook vào controller sau
- Tạm thời: Copy field Username (không cần decrypt)
- `[T]` Chọn row, nhấn Copy → paste vào text editor verify
- `[O]` Copy button hoạt động với username
- `[DoD]` Clipboard có giá trị đúng

---

#### THỨ 7

**M1**
- Review `security.py` và `generator.py` — đảm bảo tất cả function có docstring
- Kiểm tra lại Interface Contract, cập nhật nếu có thay đổi
- Commit tất cả W3 work

**M2**
- Review `database.py` — kiểm tra error handling cơ bản (conn là None, account_id không tồn tại)
- Thêm `try/except` cho `add_account()` và `delete_account()`
- Commit

**M3**
- Manual test tất cả dialogs
- Kiểm tra layout trên window size nhỏ hơn
- Commit

---

#### CHỦ NHẬT

**Cả nhóm**
- Họp nhanh 30–60 phút
- Chạy `pytest` toàn bộ — tất cả phải pass
- Review Mock Integration test — đảm bảo M3 hiểu flow encrypt/decrypt
- Confirm plan cho W4 — ai làm gì ngày nào
- `[DoD]` Tất cả test pass, cả nhóm đã chạy mock integration test thành công

---

### TUẦN 4 — First Runnable MVP

> W4 là tuần integration. Cả 3 cùng làm, không ai làm riêng module mới.

#### THỨ 2

**M1**
- `[C]` Tạo `src/app.py`. Viết class `PasswordManagerApp`:
  - `__init__`: khởi tạo DB connection, lưu derived_key = None
  - `login(master_password: str) -> bool`: lấy salt từ DB config, derive key, lưu vào `self.derived_key`
  - Nếu chưa có salt (lần đầu): generate salt, lưu vào DB config, derive key
- `[T]` Test `login("master123")` → `self.derived_key` không None
- `[O]` `app.py` với `login()` method
- `[DoD]` Login thành công, derived_key là bytes

**M2**
- `[C]` Viết `app.py` method:
  - `get_accounts() -> list[dict]`: gọi `db.get_all_accounts()`, với mỗi account decrypt `password_enc` bằng `self.derived_key`, thêm field `password_plain`
  - `add_account(service, username, password_plain, url, category)`: encrypt password_plain → gọi `db.add_account()`
- `[T]` Test bằng script: login → add_account → get_accounts → verify `password_plain` đúng
- `[O]` `get_accounts()` và `add_account()` trong `app.py`
- `[DoD]` Round-trip qua app controller hoạt động

**M3**
- `[C]` Kết nối `LoginWindow` với `app.py`:
  - Button "Login" gọi `app.login(password_entry.get())`
  - Nếu login thành công → đóng LoginWindow, mở MainWindow
  - Nếu thất bại → hiển thị Label "Sai mật khẩu" (chưa cần popup)
- `[T]` Chạy app, nhập đúng password → MainWindow mở, nhập sai → thông báo lỗi
- `[O]` Login flow hoạt động end-to-end
- `[DoD]` Đúng password → MainWindow. Sai → thông báo

---

#### THỨ 3

**M2**
- `[C]` Viết `app.py` methods:
  - `update_account(account_id, **kwargs)`: nếu password_plain trong kwargs thì encrypt trước, gọi `db.update_account()`
  - `delete_account(account_id)`: gọi `db.delete_account()`
- `[T]` Test update thay đổi username, test delete xóa đúng record
- `[O]` `update_account()` và `delete_account()` trong `app.py`
- `[DoD]` Update và delete hoạt động qua controller

**M3**
- `[C]` Kết nối `MainWindow` với `app.get_accounts()`:
  - Khi `MainWindow` mở → gọi `app.get_accounts()`, load vào Treeview
  - Treeview chỉ hiển thị: Service, Username, "••••••" (không hiển thị password plaintext)
- `[T]` Login → MainWindow → verify Treeview có data (hoặc empty nếu chưa có accounts)
- `[O]` Treeview load data từ app
- `[DoD]` Data thật từ DB hiển thị trên Treeview

**M1**
- `[C]` Kết nối nút "Add" trên MainWindow với `AddAccountDialog`:
  - Khi Save trong dialog → gọi `app.add_account()` với dữ liệu từ form
  - Sau khi add → refresh Treeview
- `[T]` Nhấn Add → dialog mở → nhập data → Save → Treeview cập nhật
- `[O]` Add flow hoạt động
- `[DoD]` Thêm account → hiện ngay trong Treeview

---

#### THỨ 4

**M1**
- `[C]` Kết nối nút "Edit" với `EditAccountDialog`:
  - Khi chọn row trong Treeview → Edit mở `EditAccountDialog` với data của row đó
  - Khi Save → gọi `app.update_account()` → refresh Treeview
- `[T]` Chọn row → Edit → thay đổi username → Save → Treeview cập nhật
- `[O]` Edit flow hoạt động
- `[DoD]` Edit account → data thay đổi trong Treeview

**M2**
- `[C]` Kết nối nút "Delete" với `app.delete_account()`:
  - Khi chọn row → Delete → hỏi confirm ("Bạn có chắc muốn xóa?") bằng `messagebox.askyesno()`
  - Nếu Yes → gọi `app.delete_account()` → refresh Treeview
- `[T]` Chọn row → Delete → Yes → Treeview không còn row đó
- `[O]` Delete flow với confirm dialog
- `[DoD]` Xóa account → biến mất khỏi Treeview, xóa thật trong DB

**M3**
- `[C]` Kết nối "Copy Password" button:
  - Khi chọn row → Copy Password → tìm account_id → gọi `app.get_account(id)` → decrypt → copy to clipboard
  - Thêm `app.get_account(account_id) -> dict` vào `app.py`
- `[T]` Chọn row → Copy → paste vào Notepad → verify đúng password
- `[O]` Copy password flow
- `[DoD]` Clipboard có đúng decrypted password

---

#### THỨ 5

**M3**
- `[C]` Kết nối Search bar:
  - Entry search trên MainWindow → khi nhập → gọi `app.search(keyword)` → reload Treeview với kết quả
  - Thêm `search(keyword: str) -> list[dict]` vào `app.py`
- `[T]` Nhập "git" → Treeview lọc chỉ còn GitHub entries
- `[O]` Search hoạt động end-to-end
- `[DoD]` Search filter đúng

**M1**
- `[C]` Kết nối Generator vào AddAccountDialog:
  - Nút "Generate" trong form → mở GeneratorDialog → chọn "Use" → điền vào Entry password
- `[T]` Full flow: Add → Generate → Use → Save → Treeview có entry mới với generated password
- `[O]` Generator integrated
- `[DoD]` Generated password được encrypt và lưu đúng

**M2**
- `[C]` Thêm xử lý "First Run":
  - Nếu `config` table không có salt → đây là lần chạy đầu tiên → generate salt mới, lưu vào DB
  - Nếu đã có salt → load salt từ DB
- `[T]` Xóa DB, chạy app lần đầu → tạo salt mới. Chạy lần 2 → dùng salt cũ
- `[O]` First-run logic trong `app.py`
- `[DoD]` Lần 1: salt được tạo và lưu. Lần 2: dùng salt cũ, decrypt đúng

---

#### THỨ 6 — Internal Demo

**Cả nhóm cùng làm**
- Chạy app từ đầu đến cuối:
  1. Khởi động app → LoginWindow
  2. Nhập Master Password → đăng nhập
  3. Xem danh sách (empty hoặc có data từ trước)
  4. Thêm account mới (dùng manual password)
  5. Thêm account mới (dùng Generator)
  6. Edit account vừa tạo
  7. Copy password, paste vào Notepad verify
  8. Search theo tên service
  9. Delete account
  10. Đóng app, mở lại → data vẫn còn (verify persistence)
- Ghi lại bugs vào GitHub Issues
- `[DoD]` Toàn bộ 10 bước chạy không crash. Bugs được ghi rõ.

---

#### THỨ 7 + CHỦ NHẬT — Bug Fix Sprint

**M1**: Fix bugs liên quan đến security/generator
**M2**: Fix bugs liên quan đến database/data flow
**M3**: Fix bugs liên quan đến UI/display

- Mỗi người fix issues của module mình
- Sau khi fix → chạy lại pytest để verify không break test cũ
- Commit fix với message rõ ràng: `"fix: [mô tả bug]"`
- `[DoD]` Tất cả bugs từ Internal Demo nghiêm trọng (crash, data loss) đã được fix

---

### TUẦN 5 — Feature Completion + Stabilization

#### THỨ 2

**M1**
- `[C]` Hiển thị password strength indicator trong AddAccountDialog và GeneratorDialog: khi nhập/generate password → gọi `check_password_strength()` → cập nhật Label màu (Red/Orange/Green) hoặc text "Weak/Medium/Strong"
- `[T]` Nhập "abc" → "Weak" màu đỏ. Nhập "Abc@12345678" → "Strong" màu xanh
- `[DoD]` Strength indicator cập nhật realtime khi nhập

**M2**
- `[C]` Thêm column "Category" vào Treeview nếu chưa có. Thêm Combobox filter trên MainWindow: "All", "Work", "Personal", "Social", "Other"
- `[T]` Add 3 accounts với 3 categories khác nhau, dùng filter → verify lọc đúng
- `[DoD]` Filter hoạt động

**M3**
- `[C]` Thêm `app.search_by_category(category)` method. Kết nối Combobox filter với method này
- `[T]` End-to-end: filter by category → Treeview cập nhật
- `[DoD]` Category filter hoạt động end-to-end

---

#### THỨ 3

**M1**
- `[C]` Integrate `check_common_password()`: trong AddAccountDialog, khi nhập password và focus-out → kiểm tra common password, hiển thị cảnh báo nhỏ nếu match
- `[T]` Nhập "password123" → cảnh báo xuất hiện
- `[DoD]` Cảnh báo hiển thị đúng

**M2**
- `[C]` Thêm `get_account(account_id) -> dict` vào `database.py` nếu chưa có. Thêm thêm field `notes` vào bảng `accounts` (migration đơn giản: ALTER TABLE hoặc recreate)
- `[T]` Thêm account với notes, verify get_account trả về notes
- `[DoD]` Field notes hoạt động

**M3**
- `[C]` Thêm field Notes vào AddAccountDialog và EditAccountDialog (Text widget nhỏ, ~3 dòng)
- `[T]` Add account với notes → Edit → notes vẫn còn
- `[DoD]` Notes lưu và hiển thị đúng

---

#### THỨ 4–5 — Bug Fix + Unit Test

**M1**: Bổ sung test cases edge case cho security + generator. Fix bugs từ W4.

**M2**: Bổ sung test cases cho database (empty keyword search, delete non-existent id, add với special chars). Fix bugs.

**M3**: Manual test tất cả UI flows: Add, Edit, Delete, Copy, Search, Filter, Generator. Ghi lại bugs.

---

#### THỨ 6–7 + CHỦ NHẬT

**Cả nhóm**
- Fix bugs từ T4–T5
- Chạy `pytest` full suite → phải pass
- Commit tất cả
- Review lại scope: MUST HAVE có xong chưa? SHOULD HAVE nào có thể bắt đầu?
- `[DoD]` Tất cả MUST HAVE features hoàn thành và ổn định

---

### TUẦN 6 — SHOULD HAVE + Consolidation

#### THỨ 2–3

**M2**
- `[C]` Viết `export_to_csv(conn, key, filepath)` trong `database.py`: get_all_accounts → decrypt từng password → write CSV với headers: service, username, password, url, category, notes
- `[T]` Export → mở file CSV verify có đủ data
- `[DoD]` CSV file đúng format, có data

**M1**
- `[C]` Thêm strength indicator cho EditAccountDialog khi xem/sửa password
- Review lại security.py — clean up, add docstrings đầy đủ
- Viết thêm tests nếu còn thiếu

**M3**
- `[C]` Thêm menu bar hoặc toolbar button "Export CSV" trong MainWindow → mở file dialog chọn đường dẫn → gọi `app.export_csv(filepath)`
- `[T]` Click Export → chọn path → file CSV được tạo
- `[DoD]` Export flow hoạt động từ UI

---

#### THỨ 4–5

**Cả nhóm**
- Integration test: test toàn bộ flow với data thật (10–15 accounts)
- Check: mở app, thêm nhiều account, search, filter, export CSV, đóng app, mở lại, data vẫn đúng
- Fix bất kỳ issue nào phát hiện

---

#### THỨ 6–7 + CHỦ NHẬT

**Cả nhóm**
- Code review nhẹ: mỗi người review code của người khác, comment nhỏ
- Cleanup: xóa code thừa, debug prints, TODO comments đã xong
- `[DoD]` Code sạch, không có debug prints, tất cả test pass

---

### TUẦN 7 — Testing & Bug Fix

> **Scope Freeze sau W7**: Không thêm feature mới sau tuần này.

#### THỨ 2–3

**M1**
- Viết thêm edge case tests cho `security.py`:
  - Empty string encrypt/decrypt
  - Very long password
  - Unicode characters
  - Wrong key type

**M2**
- Viết thêm edge case tests cho `database.py`:
  - Search với empty string
  - Update non-existent ID
  - Add account với None fields
  - SQL injection attempt (verify parameterized query an toàn)

**M3**
- Manual test plan: viết `docs/TEST_PLAN.md` — liệt kê 20+ test cases UI
- Thực hiện từng test case, ghi Pass/Fail

---

#### THỨ 4–5

**Cả nhóm**
- Viết Integration Tests trong `tests/test_integration.py`:
  - Full login flow
  - Add → get → verify encrypted
  - Search returns correct subset
  - Delete removes from DB
- Chạy full test suite

---

#### THỨ 6–7 + CHỦ NHẬT

**Cả nhóm**
- Fix tất cả bugs từ testing tuần này
- Regression test sau mỗi fix
- **Scope Freeze**: cả nhóm đồng thuận, không thêm feature mới
- Commit + tag: `git tag v0.8-scope-freeze`

---

### TUẦN 8 — Full Integration & Polish

> **Feature Freeze Day 5**

#### THỨ 2–3

**Cả nhóm**
- Full end-to-end system test với "fresh database":
  - Xóa DB cũ
  - Khởi động app lần đầu
  - Tạo 10 accounts đủ loại
  - Test tất cả MUST HAVE features
  - Test SHOULD HAVE features đã làm
  - Ghi lại bugs

---

#### THỨ 4–5

**Cả nhóm**
- Fix bugs từ system test
- **Feature Freeze Day 5**: sau T5, không sửa logic business nữa

---

#### THỨ 6–7

**M1**: Viết/hoàn thiện `docs/ARCHITECTURE.md` — giải thích security flow, module structure

**M2**: Viết/hoàn thiện `docs/REQUIREMENTS.md` — liệt kê MUST/SHOULD/COULD HAVE và trạng thái

**M3**: Cleanup UI — đảm bảo layout nhất quán, font readable, không có widget bị cắt

---

#### CHỦ NHẬT

**Cả nhóm**
- Rehearse demo: chạy toàn bộ app flow trong ~10 phút
- Mỗi người biết giải thích phần mình làm
- `[DoD]` Demo script chạy được, cả 3 có thể giải thích code của mình

---

### TUẦN 9 — Final Stabilization & Submission

> **Code Freeze Day 1**

#### THỨ 2 — Code Freeze

- Không viết code mới
- Chỉ fix critical bugs nếu có (data loss, crash khi start)
- `git tag v1.0-final`

#### THỨ 3–4 — Documentation

**M1**: Viết phần Security trong README — giải thích cách mã hóa hoạt động, tại sao an toàn

**M2**: Viết phần Installation + Usage trong README — hướng dẫn cài dependencies, chạy app

**M3**: Viết phần Features trong README — screenshot hoặc mô tả từng tính năng

**Cả nhóm**: Hoàn thiện `project/ROADMAP.md` — cập nhật trạng thái thực tế

---

#### THỨ 5 — Final Test Run

**Cả nhóm**
- Clone repo về máy mới (hoặc thư mục mới)
- Cài dependencies từ `requirements.txt`
- Chạy app từ đầu — verify chạy được không cần config thêm
- `[DoD]` App chạy được từ fresh clone

---

#### THỨ 6 — Submission Prep

- Kiểm tra `.gitignore` có exclude `*.db`, `data/passwords.db` không
- Verify không có API key, password thật nào trong code
- Final commit: `"docs: finalize README and documentation for submission"`
- Tạo Release trên GitHub nếu cần

#### THỨ 7 + CHỦ NHẬT — Buffer / Submit

- Submit
- Buffer nếu có vấn đề phút chót

---

## 9. INTEGRATION STRATEGY

### Incremental Integration Timeline

```
W2: Interface Contract định nghĩa → mỗi module có smoke test độc lập
W3 T5: Mock Integration Test (M1 + M2) — encrypt → store → decrypt với mock DB
W4: Full integration sprint — cả nhóm, mỗi người kết nối module của mình
W5: Feature integration — generator, search, filter
W6: SHOULD HAVE integration — export CSV
W7: Integration tests có hệ thống
W8: System test + final integration
```

### Nguyên tắc Integration

1. **Mỗi người kết nối module của mình** — không ai "ghép code cho người khác"
2. **app.py là controller chung** — M1, M2, M3 đều contribute vào `app.py`
3. **Mock data trước, real data sau** — test interface trước khi kết nối thật
4. **Refresh sau mỗi mutation** — sau Add/Edit/Delete luôn gọi lại load_accounts()

### Integration Checkpoint

| Tuần | Checkpoint | Ai chịu trách nhiệm |
|------|------------|---------------------|
| W2 Chủ nhật | Module smoke tests pass | Mỗi người tự verify |
| W3 T5 | Mock integration test pass | M1 + M2 cùng làm |
| W4 T6 | Internal demo 10 bước | Cả nhóm |
| W5 CN | MUST HAVE features all working | Cả nhóm |
| W7 CN | Full test suite pass | Cả nhóm |
| W8 CN | Demo rehearsal pass | Cả nhóm |

---

## 10. TESTING STRATEGY

### Unit Testing (bắt đầu từ W2)

| File | Ai viết | Khi nào |
|------|---------|---------|
| `tests/test_security.py` | M1 | W2 T6 |
| `tests/test_database.py` | M2 | W2 T6 |
| `tests/test_generator.py` | M1 | W3 T6 |

### Integration Testing

| File | Ai viết | Khi nào |
|------|---------|---------|
| `tests/test_integration_mock.py` | M1 + M2 | W3 T5 |
| `tests/test_integration.py` | Cả nhóm | W7 T4–T5 |

### Manual / System Testing

| Activity | Ai làm | Khi nào |
|----------|--------|---------|
| Internal demo | Cả nhóm | W4 T6 |
| UI test plan | M3 | W7 T2–T3 |
| System test (fresh DB) | Cả nhóm | W8 T2–T3 |
| Fresh clone test | Cả nhóm | W9 T5 |

### Regression Testing

- Sau mỗi bug fix: chạy lại `pytest` full suite
- Nếu test fail sau fix → không merge, fix tiếp

### Nguyên tắc Testing

- **Dùng in-memory SQLite** (`:memory:`) cho unit tests — nhanh, không tạo file
- **Không test UI bằng pytest** — manual test là đủ cho project này
- **Test trước commit** — không commit code chưa test

---

## 11. GITHUB WORKFLOW

### Branch Structure

```
main          ← stable, chỉ merge khi đã test
feature/*     ← new features (feature/add-account-dialog)
fix/*         ← bug fixes (fix/login-crash)
docs/*        ← documentation only
```

### Workflow mỗi task

```
1. Tạo Issue trên GitHub (mô tả task)
2. Tạo branch từ main: git checkout -b feature/ten-task
3. Code + Test local
4. Commit: git commit -m "feat: mô tả ngắn gọn"
5. Push: git push origin feature/ten-task
6. Tạo Pull Request → assign 1 người khác review
7. Review: người kia comment hoặc approve
8. Merge vào main
9. Close Issue
```

### Commit Message Format

```
feat: thêm feature mới
fix: sửa bug
test: thêm/sửa test
docs: thêm/sửa documentation
refactor: tái cấu trúc code, không thay đổi logic
chore: cập nhật .gitignore, requirements.txt, ...
```

### Đảm bảo cả 3 có contribution

- Mỗi người tạo ít nhất 2–3 Issues/tuần
- Mỗi người có commits đều đặn
- Mỗi người review ít nhất 1 PR/tuần

---

## 12. SCOPE CUT / BUFFER PLAN

### Scope Cut Priority (nếu trễ tiến độ)

```
1. Cắt COULD HAVE (Import CSV, themes, sort) → không ảnh hưởng gì
2. Cắt SHOULD HAVE cuối: Export CSV (W6) → bỏ nếu W5 chưa xong
3. Cắt SHOULD HAVE: check_common_password UI warning → giữ function nhưng bỏ UI
4. KHÔNG cắt MUST HAVE trừ khi thực sự không còn thời gian
5. KHÔNG thêm feature mới sau Scope Freeze
```

### Buffer Weeks

- **W7**: 30% thời gian dành cho buffer (testing + bug fix)
- **W8**: 20% thời gian buffer
- **W9**: 100% buffer — nếu mọi thứ xong từ W8 thì W9 chỉ là polish + submit

### Nếu W4 MVP chưa chạy được

→ Tập trung W5 fix MVP, đẩy SHOULD HAVE sang "cut"  
→ Không thêm feature mới  
→ Scope Freeze sớm hơn (sau W5 thay vì W7)

---

## 13. SCOPE FREEZE / FEATURE FREEZE / CODE FREEZE

| Milestone | Thời điểm | Ý nghĩa |
|-----------|-----------|---------|
| **Scope Freeze** | Cuối W7 (Chủ nhật) | Không thêm feature mới. Chỉ fix bugs. |
| **Feature Freeze** | W8 Day 5 (Thứ 6) | Không sửa logic business. Chỉ fix critical bugs + docs. |
| **Code Freeze** | W9 Day 1 (Thứ 2) | Không viết code mới. Chỉ fix data loss / crash khi start. |
| **Submit** | W9 Day 6–7 | Nộp bài |

---

## 14. FINAL REPOSITORY STRUCTURE

```
Password-Manager/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── src/
│   ├── security.py
│   ├── generator.py
│   ├── database.py
│   ├── app.py
│   └── ui/
│       ├── __init__.py
│       ├── login_window.py
│       ├── main_window.py
│       └── dialogs.py
│
├── tests/
│   ├── test_security.py
│   ├── test_generator.py
│   ├── test_database.py
│   ├── test_integration_mock.py
│   └── test_integration.py
│
├── data/
│   ├── common_passwords.txt
│   └── .gitkeep
│
└── docs/
    ├── INTERFACE_CONTRACT.md
    ├── ARCHITECTURE.md
    ├── REQUIREMENTS.md
    └── TEST_PLAN.md
```

**Không tạo:** `config/`, `scripts/`, `assets/`, `migrations/` — không cần thiết cho project này.

**.gitignore phải có:**
```
*.db
data/passwords.db
__pycache__/
*.pyc
.env
*.key
```

---

## 15. FINAL SELF-AUDIT

| Checklist | Status | Ghi chú |
|-----------|--------|---------|
| W1 không có main implementation | ✅ | W1 chỉ có planning/setup |
| Development bắt đầu W2 | ✅ | W2 T2 bắt đầu code |
| W2–W9 có daily task | ✅ | Từng ngày có task cụ thể cho từng member |
| Cả 3 đều code | ✅ | M1: security+gen, M2: DB, M3: UI |
| Workload cân bằng | ✅ | Đã redistribute, M3 không gánh integration |
| Member 3 không gánh integration/QA một mình | ✅ | W4 integration là trách nhiệm chung |
| Có incremental integration | ✅ | W2 contract → W3 mock → W4 MVP → W5-6 features |
| Có unit test từ sớm | ✅ | W2 T6 — cả 3 đều viết test |
| Có buffer | ✅ | W7 30% buffer, W9 là buffer tuần |
| Có scope cut | ✅ | Priority 1–5 rõ ràng |
| Có scope freeze | ✅ | Cuối W7 |
| MVP chạy được sớm | ✅ | W4 T6 internal demo |
| Không over-engineer | ✅ | Bỏ entropy, breach API, auto-lock |
| Phù hợp beginner | ✅ | Task chia nhỏ, có learning step |
| Phù hợp môn Nhập môn Python | ✅ | OOP, file, DB, GUI, testing — đúng scope |
| Có thể demo | ✅ | Demo script W8 CN |
| Có thể giải thích trong buổi bảo vệ | ✅ | Mỗi người chịu trách nhiệm module mình |
| Có thể hoàn thành trước deadline | ✅ | Code Freeze W9 D1, submit W9 D6-7 |

**Tất cả 18/18 checklist PASS.**

---

*Generated: Master Project Plan v1.0 — Password Manager Python Project*
