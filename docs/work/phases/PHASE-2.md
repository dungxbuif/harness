---
artifact_type: phase
id: PHASE-2
status: ready
owner: human
human_fields:
  - goal
  - scope
  - out_of_scope
  - priority
  - success_criteria
ai_fields:
  - risks
  - dependencies
  - verification_plan
  - completion_summary
shared_fields:
  - status
  - trace
  - tickets_and_bugs
trace:
  backlog_items: []
  roadmap: ROADMAP
  requirements: []
  tickets: []
  bugs: []
  test_verification: TBD
  validation_matrix: TBD
  adrs: []
  release_notes: TBD
---

# Phase: PHASE-2 Phát triển Harness CLI (Bộ xác thực tự động)

## Field Ownership

- Human fills goal, scope, out of scope, priority, and success criteria.
- AI fills risks, dependencies, verification plan, and completion summary.
- Shared fields include status, trace links, and ticket/bug list.

## Status

- ID: PHASE-2
- Status: ready
- Owner: human
- Created: 2026-06-05
- Updated: 2026-06-05

## Trace Links

- Backlog items: TBD
- Roadmap: [ROADMAP.md](../ROADMAP.md)
- Requirements: TBD
- Tickets: TBD
- Bugs: TBD
- Test verification: TBD
- Validation matrix: TBD
- ADRs: TBD
- Release notes: TBD

## Goal

Phát triển công cụ CLI `harness` nhằm giải quyết dứt điểm "Hệ thống danh dự" (Honor System) của LLM. Script vật lý này sẽ parse các cấu trúc YAML frontmatter đã định nghĩa và hoạt động như một "Hard Gate" (Chốt chặn cứng), không cho phép AI Agent vượt rào hoặc thực thi sai quy trình SDLC.

## Scope

- Thiết kế và phát triển `harness` CLI (có thể viết bằng Bash, Python hoặc Node.js).
- Implement lệnh `harness check-ticket <id>`:
  - Quét file YAML của ticket.
  - Xác nhận có đủ Trace links (Forward/Backward links).
  - Xác nhận Human đã set `Approved: true` (nếu bắt buộc).
  - Trả về mã lỗi (exit 1) nếu các điều kiện trên không được thỏa mãn.
- Implement lệnh `harness guard`:
  - Hook tích hợp vào quá trình chạy test (Execution & Test phase).
  - Đếm số lần chạy test/vòng lặp sửa lỗi của AI.
  - Nếu số lần test fail vượt quá giới hạn (ví dụ: > 5 lần), ném lỗi khóa màn hình, ép AI ngừng session và chờ Human review.
- Cập nhật tài liệu (README/AGENTS.md) để buộc AI phải gọi các lệnh CLI này trong Phase 2 (Detail Design) và Phase 3 (Execution).

## Out Of Scope

- Triển khai Harness Orchestrator hoàn chỉnh quản lý đa agent.
- Các tính năng CI/CD tự động trên server. (Chỉ tập trung vào IDE/Local hook).

## Tickets And Bugs

| ID | Type | Title | Status | Link |
| --- | --- | --- | --- | --- |
| TBD | Ticket | Implement `harness check-ticket` | draft | TBD |
| TBD | Ticket | Implement `harness guard` | draft | TBD |
| TBD | Ticket | Cập nhật AGENTS.md tích hợp CLI | draft | TBD |

## Dependencies

- Phụ thuộc vào cấu trúc YAML Frontmatter đã được chuẩn hóa tại Phase 1.
- Node.js hoặc Python (tuỳ thuộc vào ngôn ngữ được chọn để parse YAML dễ dàng).

## Risks

- Nếu AI vẫn có quyền "sửa" file script CLI hoặc bỏ qua việc gọi lệnh CLI, thì hệ thống vẫn tồn tại lỗ hổng. (Cần cấu hình Read-Only hoặc IDE level hook).
- Khả năng tương thích YAML parser trên các hệ điều hành khác nhau (Windows, MacOS, Linux) nếu dùng Bash thuần.

## Success Criteria

- Có thể chặn thành công một Agent đang cố gắng viết code khi `DETAIL_DESIGN` chưa được duyệt.
- Chặn thành công Agent đang rơi vào vòng lặp fix lỗi (Infinite Loop) ở chu kỳ thứ 6.
- Scripts chạy được trực tiếp trên Local Repo mà không đòi hỏi cài đặt quá phức tạp.

## Verification Plan

- Chạy thử nghiệm giả lập một Agent cố tình sinh code mà không có Approval -> `harness check-ticket` phải văng lỗi `exit 1`.
- Chạy thử nghiệm một Agent tạo vòng lặp test fail 6 lần liên tiếp -> `harness guard` phải ném lỗi block luồng thực thi.

## Gate Checklist

- [x] Phase has linked requirements or explicit discovery goal
- [ ] Tickets/bugs are created or planned
- [ ] Risks and dependencies are recorded
- [ ] Verification plan is defined
- [ ] Release/changelog need is identified

## Completion Summary

Complete this section when the phase is done.
