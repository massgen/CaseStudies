# Codebase Architecture Analysis

**Status:** 🧪 In Testing  
**Version:** v0.1.x  
**Last Updated:** November 15, 2025

## Overview

Multi-agent collaborative analysis of large codebases (FastAPI example) creating comprehensive architecture documentation by reading 30+ files through coordinated agent exploration and synthesis.

## Feature Description

### Goal
Enable multiple agents to collaboratively analyze large codebases, understand architecture, identify patterns, and generate comprehensive documentation without human guidance.

### Key Components

1. **Coordinated File Discovery**
   - Agents identify important files through README, imports, and structure analysis
   - Prioritize core components over utilities
   - Balance breadth (many files) vs. depth (thorough analysis)

2. **Distributed Reading Strategy**
   - Assign file subsets to different agents
   - Use memory system to avoid re-reading
   - Share findings through agent communication

3. **Architecture Synthesis**
   - Identify design patterns (MVC, dependency injection, etc.)
   - Map component interactions and data flows
   - Document request/response lifecycle
   - Extract key abstractions and interfaces

4. **Documentation Generation**
   - Create architecture diagrams (text-based or Mermaid)
   - Write component descriptions
   - Document key patterns and conventions
   - Generate getting-started guide for contributors

### Target: FastAPI Repository
- **Size:** ~100 Python files
- **Reading Goal:** 30+ core files
- **Output:** Comprehensive architecture document

## Test Strategy

### File Selection Tests
- Verify agents identify core files (not just tests/examples)
- Test prioritization: core > utils > tests
- Validate coverage of main architecture components

### Reading Efficiency Tests
- Measure file reads per agent
- Check for redundant reads (should use memory)
- Validate 30+ file coverage achieved
- Test with various codebase sizes

### Analysis Quality Tests
- Verify design patterns correctly identified
- Check completeness of component interaction map
- Validate data flow documentation accuracy
- Test against ground truth (FastAPI docs)

### Documentation Tests
- Review generated docs for completeness
- Check technical accuracy
- Validate usefulness for new contributors
- Compare to human-written architecture docs

### Validation Criteria
- ✅ 30+ files analyzed from FastAPI
- ✅ All major components documented
- ✅ Key patterns identified (routing, dependencies, etc.)
- ✅ <30 minutes total execution time
- ✅ Documentation useful to new contributors

## Implementation Notes

**Configuration:**
```yaml
# tools/memory/gpt5mini_gemini_codebase_analysis_memory.yaml
agents:
  - name: explorer
    role: Identify and prioritize files
    backend: gpt-5-mini
  
  - name: analyzer_1
    role: Read and analyze core components
    backend: gemini-2.0-flash
    memory: persistent
  
  - name: analyzer_2
    role: Read and analyze utilities
    backend: gemini-2.0-flash
    memory: persistent
  
  - name: synthesizer
    role: Create architecture documentation
    backend: gpt-5-mini

coordination:
  pattern: sequential
  memory_sharing: enabled
```

**Test Command:**
```bash
git clone https://github.com/tiangolo/fastapi.git
cd fastapi
massgen --config tools/memory/gpt5mini_gemini_codebase_analysis_memory.yaml \
  --query "Analyze this codebase architecture"
```

**Expected Output Structure:**
- Architecture Overview
- Component Map
- Design Patterns Used
- Request Flow Diagram
- Key Abstractions
- Getting Started for Contributors

## Related Work
- Persistent Memory (v0.1.5) - Memory system foundation
- Multi-Turn Filesystem (v0.0.25) - File access capabilities
- Parallel File Operations (v0.1.15 planned) - Will improve read performance
