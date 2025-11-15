# Framework Integrations

**Status:** 📋 Planned  
**Version:** v0.2.0+  
**Last Updated:** November 15, 2025

## Overview

Seamless integration with popular agent frameworks including LangChain, CrewAI, and custom framework adapters for ecosystem compatibility, enabling MassGen to orchestrate agents from multiple frameworks in unified workflows.

## Description

### Goal
Enable MassGen to serve as a universal orchestration layer that can coordinate agents from different frameworks (LangChain, CrewAI, AutoGPT, etc.) in a single workflow, leveraging each framework's strengths.

### Key Features

1. **LangChain Integration**
   - Import LangChain agents and chains
   - Use LangChain tools and retrievers
   - Support LangChain memory modules
   - Bidirectional data flow with LangChain components

2. **CrewAI Integration**
   - Import CrewAI crews and agents
   - Use CrewAI task definitions
   - Leverage CrewAI role-based patterns
   - Integrate CrewAI process flows

3. **Framework Adapter System**
   - Standardized adapter interface
   - Convert framework-specific APIs to MassGen format
   - Handle framework-specific features (streaming, callbacks)
   - Support custom framework plugins

4. **Unified Configuration**
   - Single YAML config for multi-framework workflows
   - Framework-agnostic agent definitions
   - Cross-framework tool sharing
   - Common memory and state management

5. **Interoperability Features**
   - Pass data between framework agents
   - Shared tool registry across frameworks
   - Unified logging and monitoring
   - Common error handling

### Example Workflow
```yaml
agents:
  - name: langchain_researcher
    framework: langchain
    type: ReActAgent
    tools: [google_search, wikipedia]
  
  - name: crewai_writer
    framework: crewai
    role: Content Writer
    goal: Write engaging articles
  
  - name: massgen_reviewer
    framework: massgen
    backend: claude-3-5-sonnet
```

## Testing Guidelines

### Test Scenarios

1. **Single Framework Test**
   - **Setup:** Workflow using only LangChain agents
   - **Test:** Execute LangChain workflow through MassGen
   - **Expected:** Identical results to native LangChain execution
   - **Validation:** Performance within 10% of native

2. **Cross-Framework Communication**
   - **Setup:** LangChain agent → MassGen agent → CrewAI agent
   - **Test:** Pass data through all three agents
   - **Expected:** Data correctly transformed and passed
   - **Validation:** No data loss, proper type conversions

3. **Tool Sharing Test**
   - **Setup:** Define tools once, use in multiple frameworks
   - **Test:** LangChain and MassGen agents both use same MCP tool
   - **Expected:** Tool works correctly in both contexts
   - **Validation:** Consistent behavior across frameworks

4. **Memory Integration Test**
   - **Setup:** LangChain agent with memory, MassGen agent reads same memory
   - **Test:** LangChain stores fact, MassGen retrieves it
   - **Expected:** Shared memory access works correctly
   - **Validation:** No memory corruption, consistent reads

5. **Error Handling Test**
   - **Setup:** Workflow where LangChain agent fails
   - **Test:** MassGen error handling and recovery
   - **Expected:** Graceful failure, proper error propagation
   - **Validation:** Clear error messages, no cascading failures

6. **Complex Multi-Framework Workflow**
   - **Setup:** 10-agent workflow using 3 different frameworks
   - **Test:** Execute research → analysis → report pipeline
   - **Expected:** All agents coordinate successfully
   - **Validation:** Output quality matches single-framework baseline

### Compatibility Testing

- Test with multiple versions of each framework
- Verify backward compatibility when frameworks update
- Test framework-specific features (streaming, callbacks, etc.)

### Performance Testing

- Measure overhead of framework adapter layer
- Compare execution time vs. native framework
- Test with varying workflow complexity

### Validation Criteria

- ✅ Support LangChain 0.2+ and CrewAI 0.1+
- ✅ <15% performance overhead vs. native execution
- ✅ 100% data type compatibility across frameworks
- ✅ Unified error handling for all frameworks
- ✅ Community adapter template documented

## Implementation Notes

### Adapter Architecture

```python
class FrameworkAdapter(ABC):
    @abstractmethod
    def initialize_agent(self, config: Dict) -> Agent:
        """Convert MassGen config to framework-specific agent"""
        pass
    
    @abstractmethod
    def execute(self, agent: Agent, query: str) -> Result:
        """Execute agent and return standardized result"""
        pass
    
    @abstractmethod
    def convert_tools(self, tools: List[Tool]) -> List[FrameworkTool]:
        """Convert MassGen tools to framework format"""
        pass
```

### Configuration Example

```yaml
frameworks:
  langchain:
    version: "0.2.0"
    imports:
      - langchain.agents
      - langchain.chains
  
  crewai:
    version: "0.1.0"
    imports:
      - crewai

agents:
  - name: research_agent
    framework: langchain
    config:
      agent_type: zero-shot-react-description
      llm: gpt-4
      tools: [search, calculator]
  
  - name: writer_agent
    framework: crewai
    config:
      role: Technical Writer
      goal: Create documentation
      backstory: Expert technical writer
```

### Integration Roadmap

1. **Phase 1:** LangChain adapter (most popular)
2. **Phase 2:** CrewAI adapter (role-based workflows)
3. **Phase 3:** AutoGPT, BabyAGI adapters
4. **Phase 4:** Community adapter template and guidelines

## Related Work

- AG2 Framework Integration (v0.0.28) - First framework integration example
- Agent Adapter System (Planned) - Backend adapter foundation
- Custom Tools (v0.1.1) - Tool integration patterns

## References

- [LangChain Documentation](https://python.langchain.com/)
- [CrewAI Documentation](https://docs.crewai.com/)
- [ROADMAP.md](https://github.com/Leezekun/MassGen/blob/main/ROADMAP.md) - Long-term vision

## Community Contributions

We welcome community-contributed adapters for additional frameworks. See the adapter template and contribution guidelines in the main repository.
