# Feature Specification: Real-time chat system with message history and user presence

**Feature Branch**: `001-real-time-chat`  
**Created**: 2025-09-24  
**Status**: Draft  
**Input**: User description: "Real-time chat system with message history and user presence"

## Execution Flow (main)
```
1. Parse user description from Input
   → If empty: ERROR "No feature description provided"
2. Extract key concepts from description
   → Identify: actors, actions, data, constraints
3. For each unclear aspect:
   → Mark with [NEEDS CLARIFICATION: specific question]
4. Fill User Scenarios & Testing section
   → If no clear user flow: ERROR "Cannot determine user scenarios"
5. Generate Functional Requirements
   → Each requirement must be testable
   → Mark ambiguous requirements
6. Identify Key Entities (if data involved)
7. Run Review Checklist
   → If any [NEEDS CLARIFICATION]: WARN "Spec has uncertainties"
   → If implementation details found: ERROR "Remove tech details"
8. Return: SUCCESS (spec ready for planning)
```

---

## ⚡ Quick Guidelines
- ✅ Focus on WHAT users need and WHY
- ❌ Avoid HOW to implement (no tech stack, APIs, code structure)
- 👥 Written for business stakeholders, not developers

### Section Requirements
- **Mandatory sections**: Must be completed for every feature
- **Optional sections**: Include only when relevant to the feature
- When a section doesn't apply, remove it entirely (don't leave as "N/A")

### For AI Generation
When creating this spec from a user prompt:
1. **Mark all ambiguities**: Use [NEEDS CLARIFICATION: specific question] for any assumption you'd need to make
2. **Don't guess**: If the prompt doesn't specify something (e.g., "login system" without auth method), mark it
3. **Think like a tester**: Every vague requirement should fail the "testable and unambiguous" checklist item
4. **Common underspecified areas**:
   - User types and permissions
   - Data retention/deletion policies  
   - Performance targets and scale
   - Error handling behaviors
   - Integration requirements
   - Security/compliance needs

---

## Clarifications

### Session 2025-09-24
- Q: Do we support only 1:1 conversations for MVP, or also group conversations? → A: 1:1 only
- Q: Which presence signals are required for MVP? → A: Online/offline only
- Q: What are the MVP limits for message size and send rate? → A: Max 2KB; 10 msgs/5s/user
- Q: How long should message history be retained? → A: 30 days (rolling)
- Q: How frequently should online/offline presence be updated to users? → A: Instantly on change (push)

## User Scenarios & Testing *(mandatory)*

### Primary User Story
As a signed-in user, I can send and receive messages in real time in a conversation and see other participants’ presence (online/offline) while being able to browse past messages in that conversation.

### Acceptance Scenarios
1. **Given** two users are in the same conversation, **When** User A sends a text message, **Then** both users see the new message appear in the conversation view without manual refresh, and the message shows a server timestamp.
2. **Given** a user opens a conversation with existing messages, **When** they scroll up, **Then** older messages load in order until the start of history (or the 30-day retention limit) is reached.
3. **Given** a user loses network connectivity during an active conversation, **When** connectivity is restored, **Then** no duplicate visible messages appear, message order remains consistent, and the latest messages since disconnection are visible.
4. **Given** a user opens their conversation list, **When** another participant becomes online or goes offline, **Then** the presence indicator updates within 1s of the status change.

### Edge Cases
- Concurrent sends from multiple users: ordering is consistent and clearly defined (by server time) in the UI.
- Long conversations: history loading remains responsive when fetching older pages of messages.
- Large messages or rapid bursts: UI remains usable; messages are neither truncated nor reordered incorrectly. [NEEDS CLARIFICATION: maximum message size and rate limits]
- Presence flapping: transient connectivity changes MUST NOT cause presence to toggle.
- Cross-device sessions: presence correctly reflects at most one "active" state while others show last seen.
- Timezones: timestamps display in the user’s locale while ordering is consistent globally.
 - History beyond 30 days is unavailable and does not surface in UI or APIs.
- Messages exceeding 2KB are rejected with a clear error and are not sent.
- Sending more than 10 messages within 5 seconds per user triggers a rate-limit error.

## Requirements *(mandatory)*

### Functional Requirements
- **FR-001**: Users MUST be able to create or access conversations and exchange text messages in real time.
- **FR-002**: The system MUST persist message history and allow users to browse earlier messages by scrolling/updating the view.
- **FR-003**: The system MUST display participant presence: online/offline only (no last seen, no typing) within active conversations.
- **FR-004**: Message ordering MUST be stable and based on a single authoritative source so that all participants see the same order.
- **FR-005**: After temporary disconnection, users MUST see all missed messages and MUST NOT see duplicate visible messages upon reconnection.
- **FR-006**: Only conversation participants MUST be able to view conversation messages and presence.
- **FR-007**: The UI MUST clearly display message timestamps and sender identity for each message.
- **FR-008**: Conversation list MUST surface recent conversations with latest message preview and online/offline presence indicators per conversation. [NEEDS CLARIFICATION: sort rules and pin/mute behavior]
- **FR-009**: Users MUST be able to start 1:1 conversations only; group conversations are out of scope for MVP.
- **FR-010**: Message size MUST be limited to 2KB; server rejects larger messages with a clear error.
- **FR-011**: Send rate MUST be limited to 10 messages per 5 seconds per user; exceeding the limit returns a rate-limit error.
- **FR-012**: Message history retention is 30 days rolling; older messages are not retrievable by users.
- **FR-013**: Presence updates MUST be pushed instantly on change and visible to other users within ≤1s.

*Ambiguities to resolve:*
- **Auth & roles**: What user types exist, and are there moderators/admins? [NEEDS CLARIFICATION]
- **Retention**: How long MUST message history be retained (time-based or count-based)? [NEEDS CLARIFICATION]
- **Search/filters**: Is message search part of MVP? [NEEDS CLARIFICATION]
- **Attachments/reactions**: Are non-text messages in scope? [NEEDS CLARIFICATION]
- **Notifications**: Are push/email/mobile notifications required? [NEEDS CLARIFICATION]

### Key Entities *(include if feature involves data)*
- **User**: identity, display name, avatar, presence status.
- **Conversation**: unique id, participants (exactly two), type: 1:1 only, created at.
- **Membership**: mapping of user to conversation for 1:1; exactly two participants; roles are not used.
- **Message**: id, conversation id, sender id, body (text), created at (server), optional edited/deleted flags. [NEEDS CLARIFICATION: edit/delete in scope]
- **Presence**: user id, status (online/offline).

---

## Review & Acceptance Checklist
*GATE: Automated checks run during main() execution*

### Content Quality
- [ ] No implementation details (languages, frameworks, APIs)
- [ ] Focused on user value and business needs
- [ ] Written for non-technical stakeholders
- [ ] All mandatory sections completed

### Requirement Completeness
- [ ] No [NEEDS CLARIFICATION] markers remain
- [ ] Requirements are testable and unambiguous  
- [ ] Success criteria are measurable
- [ ] Scope is clearly bounded
- [ ] Dependencies and assumptions identified

---

## Execution Status
*Updated by main() during processing*

- [ ] User description parsed
- [ ] Key concepts extracted
- [ ] Ambiguities marked
- [ ] User scenarios defined
- [ ] Requirements generated
- [ ] Entities identified
- [ ] Review checklist passed

---


