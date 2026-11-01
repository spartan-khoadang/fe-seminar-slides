# Dàn ý Seminar: Front-End Toàn Cảnh Cho Fresher (75 Phút)

**Mục tiêu:** Cung cấp "bản đồ tư duy" (mental model) cho các kỹ sư Front-End mới, giúp các bạn hiểu được bức tranh tổng thể về kiến trúc, cách quản lý luồng dữ liệu, tư duy tối ưu hóa hiệu suất web và các kiến thức bảo mật cơ bản.

**Thời lượng:** 75 phút (62 phút trình bày + 10 phút Q&A + 3 phút dự phòng)

---


## 1. Chủ đề 1: Hệ sinh thái Front-End & Kiến trúc (13 phút)

**Mục đích:** Giúp fresher thoát khỏi việc học vẹt framework, chuyển sang hiểu bản chất kiến trúc.

*   **Sự tiến hóa của Front-End:** 
    *   Ngày xưa: Vanilla JS / jQuery (Thao tác trực tiếp với DOM, code rối rắm khi dự án lớn).
    *   Ngày nay: Component-Based Architecture (React, Vue, Angular) - Chia nhỏ UI thành các mảnh ghép độc lập, dễ tái sử dụng.
*   **Các mô hình Rendering (Rendering Patterns):** *(Nên dùng hình ảnh minh họa)*
    *   **CSR (Client-Side Rendering):** Server trả về file HTML gần như rỗng, React/Vue chạy trên trình duyệt mới bắt đầu vẽ giao diện. (Ưu: Chuyển trang mượt. Nhược: Load lần đầu chậm, SEO kém).
    *   **SSR (Server-Side Rendering):** Giao diện được vẽ sẵn ở Server rồi mới trả về. (Ví dụ: Next.js, Nuxt.js). (Ưu: Thấy nội dung sớm, tốt cho SEO. Nhược: Tốn tài nguyên server, vận hành phức tạp hơn).
    *   **Hydration:** Quá trình JS gắn event handler vào HTML do server trả về để trang có thể tương tác.
    *   **SSG (Static Site Generation):** HTML được tạo sẵn lúc build, phù hợp với nội dung ít thay đổi như blog, docs.
*   **Vai trò của Build Tool:** 
    *   **Transpile:** Giải thích ngắn gọn tại sao không thể chạy trực tiếp file `.jsx` hay `.vue` trên trình duyệt.
    *   **Bundle:** Gom nhóm và tối ưu hóa code để đưa lên production. Công cụ phổ biến: Vite / Webpack.

---

## 2. Chủ đề 2: Render & Main Thread (7 phút)

**Mục đích:** Xây nền tảng để hiểu vì sao cần phân loại state và vì sao UI có thể bị chậm.

*   **Render và Re-render:** 
    *   State đổi thì component render lại, sau đó React cập nhật phần DOM thay đổi.
*   **Main Thread & Event Loop (mức khái niệm):** 
    *   JS chạy lâu sẽ chặn main thread, khiến UI không phản hồi.
    *   Đây là nền để hiểu INP và debounce/throttle ở Chủ đề 4.

---

## 3. Chủ đề 3: Quản lý State - Bài toán đau đầu nhất (15 phút)

**Mục đích:** Giải quyết vấn đề luồng dữ liệu trong các ứng dụng lớn, giúp fresher tránh việc lạm dụng Redux cho mọi thứ.

*   **Vấn đề Prop Drilling:** 
    *   Định nghĩa: Truyền dữ liệu qua hàng loạt component con không cần thiết chỉ để đến được component cuối cùng.
    *   Ví dụ minh họa: Component ông nội truyền data cho component cháu chắt.
    *   Thứ tự giải quyết: Đặt state đúng chỗ → Component composition → Context hoặc global store.
*   **Phân loại State (Rất quan trọng):**
    *   **UI / Local State:** Các trạng thái trên bề mặt UI (như đang mở modal nào, chữ nhập trong input là gì). Dùng `useState`.
    *   **Shared State:** Chỉ dùng `Context API` khi cần chia sẻ state xuống cây component. Lưu ý: mọi consumer sẽ re-render khi value đổi.
    *   **Server State:** Dữ liệu lấy từ API. Đây là điểm fresher hay sai lầm nhất. Giới thiệu giải pháp hiện đại (React Query, SWR, RTK Query) để handle việc caching, loading, error thay vì tự viết `useEffect` dài dòng.
    *   **Derived State:** Nếu tính được từ state có sẵn thì không tạo state mới.
*   **Global State hiện đại:** 
    *   Nhắc nhẹ về sự khác biệt giữa Redux và các giải pháp nhẹ nhàng hơn đang là trend như Zustand hay Jotai.
    *   Redux vẫn được dùng trong nhiều dự án, và Redux Toolkit đã giảm boilerplate nhiều.

---

## 4. Chủ đề 4: Web Performance 101 (15 phút)

**Mục đích:** Đưa ra các khái niệm cơ bản về hiệu suất để fresher biết cách viết code "sạch và nhanh".

*   **Core Web Vitals là gì?** (Giới thiệu 3 chỉ số chính mà Google dùng để đánh giá website):
    *   **LCP (Largest Contentful Paint):** Tốc độ hiển thị khối nội dung chính (ảnh banner, text lớn). Bao lâu thì người dùng nhìn thấy phần quan trọng nhất?
    *   **INP (Interaction to Next Paint):** Độ trễ khi tương tác (ví dụ: click vào nút nhưng 1 giây sau web mới phản hồi).
    *   **CLS (Cumulative Layout Shift):** Độ ổn định của giao diện (ví dụ: đang đọc báo thì một cái ảnh load xong đẩy chữ tụt xuống dưới).
*   **Những mẹo tối ưu (Quick Wins) cho Fresher:**
    *   Sử dụng Lazy Loading cho hình ảnh (thuộc tính `loading="lazy"`) và cho các Component ít dùng. Lưu ý: không lazy load ảnh đầu trang vì sẽ làm LCP tệ hơn.
    *   Tối ưu hóa hình ảnh (dùng định dạng WebP, nén ảnh).
    *   Tránh gọi API quá nhiều lần không cần thiết (kết nối lại với phần React Query ở trên).
*   **Debounce & Throttle:**
    *   **Debounce:** Chờ người dùng ngừng thao tác một khoảng rồi mới chạy. Dùng cho ô tìm kiếm, validate form, autosave. (Ví dụ dễ nhớ: thang máy chờ đến khi không còn người bước vào).
    *   **Throttle:** Chạy tối đa một lần trong mỗi khoảng thời gian. Dùng cho scroll, resize, kéo thả. (Ví dụ dễ nhớ: súng bắn theo nhịp, dù giữ cò liên tục).
    *   **Lỗi hay gặp:**
        *   Tạo debounce trong thân component khiến mỗi lần render sinh ra một timer mới. Cần giữ instance ổn định bằng `useRef` hoặc `useMemo`, và hủy timer khi unmount.
        *   Debounce không giải quyết race condition. Cần `AbortController`, hoặc để React Query xử lý.
        *   Debounce chỉ giảm số lần gọi, không làm một lần render nặng nhanh hơn.
*   **Demo:**
    *   Gõ vào ô tìm kiếm, so sánh số lần hàm được gọi khi không có và có debounce.
    *   Dùng tab Network và Lighthouse trong DevTools để quan sát request và đo hiệu suất.

---

## 5. Chủ đề 5: Bảo mật cơ bản: XSS, CSRF và Lưu trữ Token (12 phút)

**Mục đích:** Giúp fresher hiểu hai kiểu tấn công phổ biến và lựa chọn nơi lưu token có cân nhắc, thay vì mặc định dùng `localStorage`.

*   **Access Token & Refresh Token (1 phút):**
    *   **Access Token:** Dùng để gọi API, thời hạn ngắn (thường 5-15 phút).
    *   **Refresh Token:** Dùng để xin access token mới, thời hạn dài hơn (thường vài ngày đến vài tuần), chỉ gửi đến endpoint refresh.
*   **XSS - Cross-Site Scripting (3 phút):**
    *   **Bản chất:** JS của kẻ tấn công chạy ngay trong trang của bạn, có thể đọc `localStorage`, thao tác DOM và gửi request.
    *   React tự escape nội dung trong `{value}`. Rủi ro nằm ở những chỗ đi vòng qua cơ chế này.
    *   **Điểm dễ bị tấn công và cách phòng:**
        *   `dangerouslySetInnerHTML`, `innerHTML`: Tránh dùng; nếu bắt buộc thì sanitize bằng DOMPurify.
        *   URL do người dùng nhập (`href="javascript:..."`): Chỉ cho phép `http:` và `https:`.
        *   Script và package bên thứ ba: Hạn chế dùng, kiểm tra dependency bằng `npm audit`.
        *   Lỗi còn lọt qua: Dùng CSP để giới hạn script được phép chạy.
    *   **Demo (chỉ chạy trên app local, dùng `console.log` thay cho việc gửi token ra ngoài):**
        *   Ô bình luận dùng `dangerouslySetInnerHTML` so với `{comment}` thông thường.
        *   `localStorage.getItem('token')` so với `document.cookie` khi cookie có `HttpOnly`.
*   **CSRF - Cross-Site Request Forgery (3 phút):**
    *   **Bản chất:** Trang độc hại khiến trình duyệt của nạn nhân gửi request đến site của bạn, và trình duyệt có thể tự đính kèm cookie.
    *   **Ví dụ minh họa:** Form tự động submit đến `https://bank.example/transfer`.
    *   **Cách phòng:**
        *   Set rõ `SameSite` là `Lax` hoặc `Strict` cho cookie.
        *   Dùng CSRF token.
        *   Kiểm tra header `Origin`.
        *   Không đổi dữ liệu bằng GET.
    *   **Hiểu lầm cần đính chính:** CORS không chống được CSRF.
*   **Lưu Token ở đâu? (3 phút):** *(Nên dùng bảng so sánh)*
    *   **`localStorage`:** Tránh được CSRF, nhưng XSS đọc được token. Còn sau khi tải lại trang.
    *   **`sessionStorage`:** Tránh được CSRF, nhưng XSS đọc được token. Mất khi đóng tab.
    *   **Biến trong memory:** Tránh được CSRF, XSS khó đọc hơn nhưng không tuyệt đối. Mất khi tải lại trang.
    *   **Cookie `HttpOnly`:** JS không đọc được, nhưng phải phòng CSRF. Còn sau khi tải lại trang.
    *   **Cách nên dùng cho SPA:**
        *   Access token: Lưu trong memory, gửi qua header `Authorization`.
        *   Refresh token: Lưu trong cookie `HttpOnly; Secure; SameSite`, giới hạn `Path` ở endpoint refresh và bật rotation.
        *   Khi tải lại trang: Gọi `/refresh` để lấy access token mới.
        *   Backend phải phối hợp, vì cookie do server set. Nếu FE và API khác site, cần `SameSite=None` và phải chống CSRF kỹ hơn.
    *   Nhắc tên **BFF (Backend for Frontend):** Token nằm hẳn ở server, trình duyệt chỉ giữ session cookie. An toàn hơn nhưng phức tạp hơn.
    *   **Thông điệp chính:** Không có nơi lưu nào an toàn tuyệt đối. `HttpOnly` chỉ ngăn JS đọc cookie, nên vẫn phải chống XSS từ gốc.
*   **Luồng Refresh & Lỗi hay gặp (2 phút):**
    *   Luồng cơ bản: API trả về 401 → Gọi `/refresh` → Gọi lại request ban đầu. Nếu refresh thất bại thì chuyển về trang login.
    *   Nhiều request cùng nhận 401: Chỉ được refresh một lần, các request còn lại chờ kết quả.
    *   Vòng lặp vô hạn: Nếu chính `/refresh` trả về 401 thì không được refresh tiếp.
    *   Logout chỉ ở phía client: Cần gọi server để thu hồi refresh token.
    *   Lộ token: Không ghi token vào log hoặc đưa lên URL.

---

## 6. Q&A và Tổng kết (10 phút)

*   **Tổng kết nhanh (1 phút):** Nhấn mạnh lại việc tư duy kiến trúc và công cụ phù hợp quan trọng hơn việc thuộc lòng cú pháp.
*   **Hỏi đáp (9 phút):** Mở không gian cho các bạn fresher đặt câu hỏi thực tế.
*   **Tài liệu tham khảo (Takeaways):** Chia sẻ QR code hoặc link (Notion/Slide) chứa các tài liệu tự học sâu hơn về các chủ đề này.
