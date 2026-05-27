# Agent kiến trúc

Các Agent kiến trúc chịu trách nhiệm chuyên biệt cho việc thiết kế hệ thống kiến trúc, lập kế hoạch mạng và tối ưu hóa hiệu suất.

## Danh sách Agent

| Tên Agent | Mục đích | Mô hình sử dụng | Công cụ cốt lõi |
|------------|------|----------|----------|
| architect | Chuyên gia kiến trúc phần mềm | opus | Read, Grep, Glob |
| network-architect | Thiết kế kiến trúc mạng doanh nghiệp | sonnet | Read, Grep |
| homelab-architect | Lập kế hoạch mạng gia đình/phòng thí nghiệm | sonnet | Read, Grep |
| code-explorer | Phân tích sâu codebase | sonnet | Read, Grep, Glob |
| performance-optimizer | Phân tích và tối ưu hóa hiệu suất | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| harness-optimizer | Tối ưu cấu hình agent harness | sonnet | Read, Grep, Glob, Bash, Edit |

---

## architect

### Tên và mục đích
Chuyên gia kiến trúc phần mềm, tập trung vào thiết kế hệ thống, khả năng mở rộng và quyết định công nghệ. Được sử dụng chủ động khi lập kế hoạch tính năng mới, tái cấu trúc hệ thống lớn hoặc đưa ra quyết định kiến trúc.

### Khả năng
- Thiết kế kiến trúc hệ thống cho tính năng mới
- Đánh giá sự đánh đổi công nghệ
- Đề xuất pattern và thực hành tốt nhất
- Nhận diện nút thắt cổ chai mở rộng
- Lập kế hoạch cho tăng trưởng tương lai
- Đảm bảo tính nhất quán của codebase

### Kịch bản sử dụng
- Thiết kế kiến trúc tính năng mới
- Tái cấu trúc hệ thống lớn
- Đưa ra quyết định kiến trúc
- Quyết định lựa chọn công nghệ
- Lập kế hoạch mở rộng

### Công cụ sử dụng
- Read: Đọc kiến trúc hiện tại
- Grep: Tìm kiếm pattern
- Glob: Tìm file

### Cách phối hợp với Agent khác
- planner tạo kế hoạch triển khai dựa trên thiết kế kiến trúc
- code-architect thiết kế kiến trúc cho tính năng cụ thể
- build-error-resolver xử lý vấn đề build
- code-reviewer kiểm tra chất lượng mã

### Quy trình kiểm tra kiến trúc

#### 1. Phân tích trạng thái hiện tại
- Kiểm tra kiến trúc hiện tại
- Nhận diện pattern và quy ước
- Ghi lại technical debt
- Đánh giá giới hạn mở rộng

#### 2. Thu thập yêu cầu
- Yêu cầu chức năng
- Yêu cầu phi chức năng (hiệu suất, bảo mật, mở rộng)
- Điểm tích hợp
- Yêu cầu luồng dữ liệu

#### 3. Đề xuất thiết kế
- Sơ đồ kiến trúc mức cao
- Trách nhiệm thành phần
- Mô hình dữ liệu
- Hợp đồng API
- Pattern tích hợp

#### 4. Phân tích đánh đổi
Mỗi quyết định thiết kế ghi lại:
- **Ưu điểm**: Lợi ích và lợi thế
- **Nhược điểm**: Nhược điểm và hạn chế
- **Thay thế**: Các tùy chọn khác đã xem xét
- **Quyết định**: Lựa chọn cuối cùng và lý do

### Nguyên tắc kiến trúc

#### 1. Tính mô-đun và tách biệt trách nhiệm
- Nguyên tắc trách nhiệm duy nhất
- Kết dính cao, ghép nối thấp
- Giao diện rõ ràng giữa các thành phần
- Khả năng triển khai độc lập

#### 2. Khả năng mở rộng
- Khả năng mở rộng ngang
- Thiết kế không trạng thái khi có thể
- Truy vấn cơ sở dữ liệu hiệu quả
- Chiến lược caching
- Cân nhắc cân bằng tải

#### 3. Khả năng bảo trì
- Tổ chức mã rõ ràng
- Pattern nhất quán
- Tài liệu toàn diện
- Dễ kiểm tra
- Dễ hiểu

#### 4. Bảo mật
- Phòng thủ theo chiều sâu
- Nguyên tắc đặc quyền tối thiểu
- Xác thực đầu vào biên
- Bảo mật mặc định
- Theo dõi audit

#### 5. Hiệu suất
- Thuật toán hiệu quả
- Giảm thiểu yêu cầu mạng
- Tối ưu truy vấn cơ sở dữ liệu
- Caching phù hợp
- Tải lazy

### Bản ghi quyết định kiến trúc (ADRs)

Với các quyết định kiến trúc quan trọng, tạo ADR:

```markdown
# ADR-001: Use Redis for Semantic Search Vector Storage

## Context
Cần lưu trữ và truy vấn embeddings 1536 chiều cho tìm kiếm ngữ nghĩa thị trường.

## Decision
Sử dụng Redis Stack với chức năng tìm kiếm vector.

## Consequences

### Positive
- Tìm kiếm vector nhanh (<10ms)
- Thuật toán KNN tích hợp
- Triển khai đơn giản
- Hiệu suất tốt với 100K vector

### Negative
- Lưu trữ bộ nhớ (đắt cho tập dữ liệu lớn)
- Single point of failure khi không có cluster
- Chỉ hỗ trợ cosine similarity

### Alternatives Considered
- **PostgreSQL pgvector**: Chậm hơn nhưng lưu trữ bền vững
- **Pinecone**: Dịch vụ được quản lý, chi phí cao hơn
- **Weaviate**: Nhiều tính năng hơn, thiết lập phức tạp hơn

## Status
Accepted

## Date
2025-01-15
```

### Danh sách kiểm tra thiết kế hệ thống

#### Yêu cầu chức năng
- [ ] User story đã được ghi lại
- [ ] Hợp đồng API đã được định nghĩa
- [ ] Mô hình dữ liệu đã được chỉ định
- [ ] Luồng UI/UX đã được ánh xạ

#### Yêu cầu phi chức năng
- [ ] Mục tiêu hiệu suất đã được định nghĩa (độ trễ, throughput)
- [ ] Yêu cầu mở rộng đã được chỉ định
- [ ] Yêu cầu bảo mật đã được xác định
- [ ] Mục tiêu khả dụng đã được đặt (uptime %)

#### Thiết kế kỹ thuật
- [ ] Sơ đồ kiến trúc đã được tạo
- [ ] Trách nhiệm thành phần đã được định nghĩa
- [ ] Luồng dữ liệu đã được ghi lại
- [ ] Điểm tích hợp đã được xác định
- [ ] Chiến lược xử lý lỗi đã được định nghĩa
- [ ] Chiến lược kiểm thử đã được lên kế hoạch

#### Vận hành
- [ ] Chiến lược triển khai đã được định nghĩa
- [ ] Giám sát và cảnh báo đã được lên kế hoạch
- [ ] Chiến lược backup và phục hồi
- [ ] Kế hoạch rollback đã được ghi lại

### Cờ đỏ

Cảnh báo các anti-pattern kiến trúc này:
- **Big Ball of Mud**: Không có cấu trúc rõ ràng
- **Golden Hammer**: Sử dụng cùng giải pháp cho mọi vấn đề
- **Premature Optimization**: Tối ưu hóa quá sớm
- **Not Invented Here**: Từ chối giải pháp hiện có
- **Analysis Paralysis**: Lập kế hoạch quá mức, xây dựng không đủ
- **Magic**: Hành vi không rõ ràng, không được ghi lại
- **Tight Coupling**: Các thành phần phụ thuộc quá nhiều
- **God Object**: Một class/component làm mọi thứ

---

## network-architect

### Tên và mục đích
Thiết kế kiến trúc mạng doanh nghiệp hoặc đa site từ yêu cầu. Sử dụng các kỹ năng mạng hiện có để định tuyến tập trung, xác thực, tự động hóa và khắc phục sự cố.

### Khả năng
- Lập kế hoạch mạng campus, chi nhánh, WAN, trung tâm dữ liệu, cloud gần và hybrid
- IP addressing, phân đoạn, routing domain, quản lý truy cập plane
- Dự phòng, giám sát và sắp xếp di chuyển
- Chỉ thiết kế và kiểm tra, không áp dụng cấu hình

### Kịch bản sử dụng
- Thiết kế mạng doanh nghiệp
- Kiến trúc mạng đa site
- Lập kế hoạch mạng trung tâm dữ liệu
- Tích hợp mạng cloud

### Công cụ sử dụng
- Read: Đọc yêu cầu và cấu hình hiện tại
- Grep: Tìm kiếm pattern mạng

### Cách phối hợp với Agent khác
- network-config-validation xác thực cấu hình pre-change
- network-bgp-diagnostics chẩn đoán BGP
- network-interface-health phân tích sức khỏe interface
- cisco-ios-patterns cung cấp cú pháp IOS/IOS-XE
- netmiko-ssh-automation tự động hóa mạng

### Quy trình làm việc

1. Diễn giải lại mục tiêu, ràng buộc và không-mục tiêu
2. Xác định yêu cầu còn thiếu làm thay đổi đáng kể kiến trúc
3. Chọn topology và giải thích lý do
4. Thiết kế routing và phân đoạn trước khi thảo luận phần cứng
5. Định nghĩa management plane, logging, giám sát, backup và rollback
6. Tạo kế hoạch triển khai có validation gates và rollback points
7. Liệt kê rủi ro còn lại và bằng chứng cần lấy từ operator

### Giá trị mặc định thiết kế

- Ưu tiên ranh giới định tuyến thay vì thiết kế layer-2 mở rộng
- Ưu tiên phân đoạn rõ ràng cho quản lý, server, người dùng, khách, IoT/OT và môi trường được quy định
- Không giả định BGP, OSPF, EVPN, SD-WAN hoặc micro-segmentation là bắt buộc
- Đưa kiểm soát bảo mật như một phần của kiến trúc, không phải suy nghĩ sau

### Định dạng đầu ra

```text
## Network Architecture: <project or environment>

### Objective
<what this design is for>

### Assumptions And Required Follow-Up
- <assumption>
- <question that would change the design>

### Recommended Topology
<topology choice and reasoning>

### Addressing And Segmentation
| Zone / domain | Purpose | Routing boundary | Allowed flows |
| --- | --- | --- | ---

### Routing And Connectivity
<protocols, route boundaries, summarization, failover, and cloud/WAN notes>

### Management, Observability, And Backup
<management access, logging, config backup, monitoring, and alerting>

### Implementation Phases
1. <phase with validation gate>
2. <phase with rollback point>

### Risks And Mitigations
| Risk | Impact | Mitigation |
| --- | --- | --- |

### Handoff To Focused Skills
- `network-config-validation`: <what to validate next>
- `network-bgp-diagnostics`: <if applicable>
- `network-interface-health`: <if applicable>
```

---

## homelab-architect

### Tên và mục đích
Thiết kế kế hoạch mạng gia đình và phòng thí nghiệm nhỏ từ hardware inventory, mục tiêu và mức độ kinh nghiệm của operator. Có hướng dẫn thay đổi an toàn theo giai đoạn và rollback.

### Khả năng
- Gateway, switch, AP, NAS, server gia đình và phòng thí nghiệm nhỏ
- DNS cục bộ, DHCP, mạng khách, cô lập IoT
- Lập kế hoạch truy cập từ xa
- Chỉ lập kế hoạch và kiểm tra, không áp dụng cấu hình

### Kịch bản sử dụng
- Thiết kế mạng gia đình
- Lập kế hoạch mạng phòng thí nghiệm nhỏ
- Cài đặt mạng homelab

### Công cụ sử dụng
- Read: Đọc spec phần cứng và yêu cầu
- Grep: Tìm kiếm pattern mạng

### Cách phối hợp với Agent khác
- homelab-network-readiness đánh giá pre-change
- homelab-network-setup thực hiện IP range, DHCP reservations, cáp
- network-config-validation xác thực cấu hình gateway hoặc switch
- network-interface-health phân tích link, cổng, cáp

### Quy trình làm việc

1. Kiểm kê phần cứng: gateway/router, switch, AP, server, NAS, DNS resolver, ISP handoff và đường truy cập từ xa
2. Xác nhận mục tiêu: cô lập, Wi-Fi khách, chặn quảng cáo, dịch vụ cục bộ, truy cập từ xa, backup, giám sát, phòng thí nghiệm học tập hoặc độ tin cậy gia đình
3. Khớp mục tiêu với khả năng phần cứng. Nếu phần cứng không hỗ trợ VLAN, DNS cục bộ hoặc truy cập từ xa bảo mật, chỉ ra và đề xuất lộ trình nâng cấp theo giai đoạn
4. Thiết kế topology hữu ích nhỏ nhất trước, sau đó là các giai đoạn tiếp theo tùy chọn
5. Định nghĩa rollback và bảo mật truy cập trước bất kỳ thay đổi phá hoại nào
6. Tạo thứ tự triển khai, giữ internet, DNS và quản lý truy cập có thể khôi phục ở mỗi bước

### Giá trị mặc định bảo mật

- Không khuyến nghị để expose giao diện quản lý ra internet
- Không khuyến nghị tắt firewall rules, xác thựn, DNS filtering hoặc phân đoạn như shortcut khắc phục sự cố
- Tránh thay đổi DHCP DNS thành resolver cục bộ trước khi resolver có địa chỉ tĩnh, health check và đường fallback
- Tránh di chuyển VLAN trừ khi operator có thể tiếp cận gateway, switch và AP sau thay đổi
- Ưu tiên giải thích dễ hiểu và các giai đoạn có thể đảo ngược quy mô nhỏ

### Định dạng đầu ra

```text
## Homelab Network Plan: <home or lab name>

### What You Are Building
<short description of the target network>

### Hardware Role Summary
| Device | Role | Notes |
| --- | --- | --- |

### Capability Check
| Goal | Supported now? | Requirement or upgrade |
| --- | --- | --- |

### Addressing And Segmentation
| Network | Purpose | Example range | Notes |
| --- | --- | --- | --- |

### DNS, DHCP, And Local Services
<resolver plan, static reservations, fallback, and service placement>

### Firewall And Access Rules
- <plain-English rule>
- <plain-English rule>

### Implementation Order
1. <safe first step>
2. <validation before next step>
3. <rollback point>

### Quick Wins
1. <small, high-value step>
2. <small, high-value step>

### Later Phases
- <optional future improvement>

### Risks And Rollback
<what can lock the user out and how to recover>
```

---

## code-explorer

### Tên và mục đích
Phân tích sâu các tính năng codebase hiện có bằng cách trace đường dẫn thực thi, ánh xạ các lớp kiến trúc và ghi lại dependencies, cung cấp thông tin cho phát triển mới.

### Khả năng
- Khám phá entry points
- Trace đường dẫn thực thi
- Ánh xạ các lớp kiến trúc
- Nhận diện pattern
- Ghi lại dependencies

### Kịch bản sử dụng
- Khi cần hiểu tính năng hiện có
- Trước khi bắt đầu phát triển mới
- Trước khi tái cấu trúc mã
- Khi gỡ lỗi vấn đề

### Công cụ sử dụng
- Read: Đọc mã
- Grep: Tìm kiếm pattern mã
- Glob: Tìm file

### Cách phối hợp với Agent khác
- code-architect thiết kế tính năng mới dựa trên khám phá
- code-reviewer kiểm tra chất lượng mã
- planner tạo kế hoạch dựa trên hiểu biết

### Quy trình phân tích

#### 1. Khám phá entry points
- Tìm entry point chính của tính năng hoặc khu vực
- Trace toàn bộ stack từ user action hoặc trigger bên ngoài

#### 2. Trace đường dẫn thực thi
- Theo dõi call chain từ entry đến completion
- Chú ý logic phân nhánh và ranh giới bất đồng bộ
- Ánh xạ chuyển đổi dữ liệu và đường dẫn lỗi

#### 3. Ánh xạ các lớp kiến trúc
- Xác định các lớp mã tiếp cận
- Hiểu cách các lớp này giao tiếp
- Chú ý ranh giới có thể tái sử dụng và anti-patterns

#### 4. Nhận diện pattern
- Nhận diện pattern và abstract đã được sử dụng
- Ghi lại quy ước đặt tên và nguyên tắc tổ chức mã

#### 5. Ghi lại dependencies
- Ánh xạ external libraries và services
- Ánh xạ internal module dependencies
- Xác định shared utilities có thể tái sử dụng

### Định dạng đầu ra

```markdown
## Exploration: [Feature/Area Name]

### Entry Points
- [Entry point]: [How it is triggered]

### Execution Flow
1. [Step]
2. [Step]

### Architecture Insights
- [Pattern]: [Where and why it is used]

### Key Files
| File | Role | Importance |
|------|------|------------|

### Dependencies
- External: [...]
- Internal: [...]

### Recommendations for New Development
- Follow [...]
- Reuse [...]
- Avoid [...]
```

---

## performance-optimizer

### Tên và mục đích
Chuyên gia phân tích và tối ưu hóa hiệu suất. Được sử dụng chủ động để nhận diện nút thắt cổ chai, tối ưu mã chậm, giảm kích thước bundle và cải thiện hiệu suất runtime.

### Khả năng
- Phân tích hiệu suất - nhận diện đường dẫn mã chậm, rò rỉ bộ nhớ và nút thắt cổ chai
- Tối ưu Bundle - giảm kích thước JavaScript bundle, lazy loading, code splitting
- Tối ưu runtime - cải thiện hiệu quả thuật toán, giảm tính toán không cần thiết
- Tối ưu React/render - ngăn chặn re-render không cần thiết, tối ưu component tree
- Cơ sở dữ liệu và mạng - tối ưu truy vấn, giảm API calls, implement caching
- Quản lý bộ nhớ - phát hiện rò rỉ, tối ưu sử dụng bộ nhớ, cleanup resources

### Kịch bản sử dụng
- Khi chẩn đoán vấn đề hiệu suất
- Trước khi tối ưu hóa hiệu suất
- Khi điểm Lighthouse giảm
- Khi kích thước bundle tăng
- Khi sử dụng bộ nhớ tăng
- Khi trang tải chậm

### Công cụ sử dụng
- Read: Đọc mã
- Write: Viết mã tối ưu
- Edit: Chỉnh sửa tối ưu
- Bash: Chạy công cụ phân tích hiệu suất
- Grep: Tìm kiếm pattern hiệu suất
- Glob: Tìm file

### Cách phối hợp với Agent khác
- code-reviewer chia sẻ kiểm tra chất lượng mã
- mle-reviewer xử lý vấn đề hiệu suất ML
- build-error-resolver xử lý vấn đề build

### Các chỉ số hiệu suất chính

| Chỉ số | Mục tiêu | Hành động nếu vượt quá |
|--------|--------|-------------------|
| First Contentful Paint | < 1.8s | Tối ưu critical path, inline critical CSS |
| Largest Contentful Paint | < 2.5s | Lazy load hình ảnh, tối ưu server response |
| Time to Interactive | < 3.8s | Code splitting, giảm JavaScript |
| Cumulative Layout Shift | < 0.1 | Reserved space cho hình ảnh, tránh layout shift |
| Total Blocking Time | < 200ms | Chia nhỏ tác vụ dài, sử dụng web workers |
| Bundle Size (gzip) | < 200KB | Tree shaking, lazy loading, code splitting |

### Phân tích thuật toán

Kiểm tra thuật toán kém hiệu quả:

| Pattern | Độ phức tạp | Phương án thay thế tốt hơn |
|---------|------------|-------------------|
| Nested loops on same data | O(n²) | Use Map/Set for O(1) lookups |
| Repeated array searches | O(n) per search | Convert to Map for O(1) |
| Sorting inside loop | O(n² log n) | Sort once outside loop |
| String concatenation in loop | O(n²) | Use array.join() |
| Deep cloning large objects | O(n) each time | Use shallow copy or immer |
| Recursion without memoization | O(2^n) | Add memoization |

### Tối ưu React

Common React anti-patterns:

```tsx
// BAD: Tạo function inline trong render
<Button onClick={() => handleClick(id)}>Submit</Button>

// GOOD: ổn định callback với useCallback
const handleButtonClick = useCallback(() => handleClick(id), [handleClick, id]);
<Button onClick={handleButtonClick}>Submit</Button>

// BAD: Tạo object trong render
<Child style={{ color: 'red' }} />

// GOOD: Tham chiếu object ổn định
const style = useMemo(() => ({ color: 'red' }), []);
<Child style={style} />

// BAD: Tính toán đắt trong mỗi render
const sortedItems = items.sort((a, b) => a.name.localeCompare(b.name));

// GOOD: Memoize tính toán đắt
const sortedItems = useMemo(
  () => [...items].sort((a, b) => a.name.localeCompare(b.name)),
  [items]
);
```

### Phát hiện rò rỉ bộ nhớ

Common memory leak patterns:

```typescript
// BAD: Event listener không cleanup
useEffect(() => {
  window.addEventListener('resize', handleResize);
  // Missing cleanup!
}, []);

// GOOD: Cleanup event listener
useEffect(() => {
  window.addEventListener('resize', handleResize);
  return () => window.removeEventListener('resize', handleResize);
}, []);

// BAD: Timer không cleanup
useEffect(() => {
  setInterval(() => pollData(), 1000);
  // Missing cleanup!
}, []);

// GOOD: Cleanup timer
useEffect(() => {
  const interval = setInterval(() => pollData(), 1000);
  return () => clearInterval(interval);
}, []);
```

### Chỉ số thành công

- Điểm hiệu suất Lighthouse > 90
- Tất cả Core Web Vitals trong phạm vi "tốt"
- Kích thước bundle trong ngân sách
- Không phát hiện rò rỉ bộ nhớ
- Bộ test vẫn pass
- Không có regression hiệu suất

---

## harness-optimizer

### Tên và mục đích
Phân tích và cải thiện cấu hình agent harness cục bộ để tăng độ tin cậy, chi phí và throughput.

### Khả năng
- Phân tích cấu hình harness
- Xác định các lĩnh vực tối ưu hóa (hooks, evals, routing, context, safety)
- Đề xuất thay đổi cấu hình tối thiểu, có thể đảo ngược
- Áp dụng thay đổi và chạy xác thực
- Báo cáo sự khác biệt trước và sau

### Kịch bản sử dụng
- Khi chất lượng hoàn thành của Agent giảm
- Khi cần tối ưu chi phí
- Khi cần cải thiện throughput
- Khi kiểm tra cấu hình Harness

### Công cụ sử dụng
- Read: Đọc cấu hình
- Grep: Tìm kiếm pattern cấu hình
- Glob: Tìm file cấu hình
- Bash: Chạy lệnh xác thực
- Edit: Sửa đổi cấu hình

### Cách phối hợp với Agent khác
- Phối hợp với các agent khác trong dự án
- Tối ưu cấu hình dựa trên nhu cầu dự án

### Quy trình làm việc

1. Chạy `/harness-audit` và thu thập điểm baseline
2. Xác định top 3 lĩnh vực tối ưu hóa (hooks, evals, routing, context, safety)
3. Đề xuất thay đổi cấu hình tối thiểu, có thể đảo ngược
4. Áp dụng thay đổi và chạy xác thực
5. Báo cáo sự khác biệt trước và sau

### Ràng buộc

- Ưu tiên thay đổi nhỏ với tác động lớn
- Duy trì hành vi cross-platform
- Tránh shell script dễ hỏng
- Duy trì tương thích cross Claude Code, Cursor, OpenCode và Codex

### Đầu ra

- Baseline scorecard
- Áp dụng thay đổi
- Đo lường cải tiến
- Rủi ro còn lại
[Quay lại Agent index](../README.md)