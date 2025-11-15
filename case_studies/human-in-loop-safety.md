# Human-in-the-Loop Safety for Irreversible Actions

**Status:** 🔄 In Progress  
**Version:** Future  
**Last Updated:** November 15, 2025

## Overview

Human approval mechanism for dangerous operations (file deletion, system commands, API calls), preventing accidental damage while maintaining agent autonomy for safe operations.

## Feature Description

### Goal
Add a safety layer that pauses execution before irreversible actions, allowing humans to review and approve/reject dangerous operations without disrupting the multi-agent workflow.

### Key Components

1. **Danger Classification System**
   - Categorize operations by risk level (safe, warning, dangerous)
   - Pattern matching for destructive commands (`rm -rf`, `DROP TABLE`, etc.)
   - Context-aware risk assessment (production vs. sandbox)

2. **Approval Workflow**
   - Pause agent execution before dangerous operation
   - Display operation details and potential impact
   - Accept/Reject/Modify interface for human review
   - Timeout and fallback behavior for unattended runs

3. **Audit Trail**
   - Log all approval requests and decisions
   - Track who approved what and when
   - Enable post-mortem analysis of incidents

4. **Configurable Safety Levels**
   - Strict: Require approval for all risky operations
   - Moderate: Auto-approve low-risk, prompt for high-risk
   - Permissive: Log only, no blocking (for trusted environments)

### Protected Operations
- File system: deletion, overwrite, permission changes
- System commands: sudo, service management, network config
- API calls: POST/DELETE to production endpoints
- Database: schema changes, data deletion
- MCP tools: irreversible external actions

## Test Strategy

### Functional Tests
- Verify approval prompt appears for dangerous operations
- Test accept/reject/modify flows
- Validate timeout behavior
- Confirm safe operations proceed without prompts

### Security Tests
- Attempt bypass through command obfuscation
- Test privilege escalation scenarios
- Verify audit log integrity

### Usability Tests
- Measure time to review and approve
- Test with real case studies (filesystem, code execution)
- Gather feedback on false positive rate

### Validation Criteria
- ✅ Zero false negatives (no dangerous ops slip through)
- ✅ <10% false positive rate (minimal disruption)
- ✅ <5 second approval flow for common operations
- ✅ Full audit trail for compliance

## Implementation Notes

**Integration Points:**
- MCP tool execution layer
- File system manager
- Shell command execution (bash_20250124)
- Custom tool invocation

**Configuration Example:**
```yaml
safety:
  level: moderate
  protected_operations:
    - file_deletion
    - system_commands
    - production_api_calls
  timeout_seconds: 300
  fallback: reject
```

See [ROADMAP.md](https://github.com/Leezekun/MassGen/blob/main/ROADMAP.md) for detailed development track.

## Related Work
- MCP Planning Mode (v0.0.29) - Preview tool usage without execution
- Revert Feature After Final Agent Failure (Issue #325) - Automated recovery
