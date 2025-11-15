# Map-Reduce Document Processing

**Status:** 📋 Planned  
**Version:** Future  
**Last Updated:** November 15, 2025

## Overview

Assign one document per agent to process, vote on which documents require manager attention. Applications include meeting notes prioritization, conference paper selection, email triage, and information-to-attention prediction for busy managers and researchers.

## Description

### Goal
Transform the problem of "which answer is better?" to "which document requires attention?" by using parallel agents to process large document sets and intelligently prioritize items that need human review.

### Key Features

1. **Parallel Document Processing**
   - One agent per document for maximum parallelization
   - Independent analysis without coordination overhead
   - Handle 100+ documents simultaneously
   - Support various formats: PDF, emails, notes, papers

2. **Intelligent Prioritization**
   - Each agent votes: "Requires Attention" or "Can Skip"
   - Customizable voting criteria per use case
   - Confidence scoring for each recommendation
   - Reasoning explanation for decisions

3. **User-Defined Criteria**
   - Meeting notes: Action items, decisions, conflicts
   - Papers: Relevance, novelty, methodology quality
   - Emails: Urgency, importance, requires response
   - News: Impact on business, actionable insights

4. **Aggregation & Ranking**
   - Coordinator agent collects all votes
   - Rank documents by priority score
   - Generate executive summary
   - Highlight top N items for review

5. **Scalable Architecture**
   - Map phase: Parallel document analysis
   - Reduce phase: Aggregate and rank
   - Efficient resource utilization
   - Cost-effective for large volumes

### Use Cases

**Meeting Notes Triage (for managers):**
- Process: 50 meeting transcripts from past week
- Criteria: Contains decisions, action items, or conflicts
- Output: Top 10 meetings requiring follow-up

**Conference Paper Selection (for researchers):**
- Process: 100 papers from arXiv/conferences
- Criteria: Novel methods, relevant to research area, strong results
- Output: Top 15 papers worth detailed reading

**Email Management (for executives):**
- Process: 200 emails from past 10 days
- Criteria: Urgent, requires decision, high-impact
- Output: Top 20 emails requiring immediate attention

**Stock News Analysis (for traders):**
- Process: 500 news articles about market
- Criteria: Material impact, actionable insights, time-sensitive
- Output: Top 30 articles affecting trading decisions

## Testing Guidelines

### Test Scenarios

1. **Small Set Test (10 documents)**
   - **Input:** 10 meeting notes, 3 contain action items
   - **Test:** Agents identify which require attention
   - **Expected:** Correctly identify all 3, rank them at top
   - **Validation:** Precision and recall both >90%

2. **Medium Set Test (50 documents)**
   - **Input:** 50 research papers, 10 highly relevant
   - **Test:** Parallel processing and ranking
   - **Expected:** Complete in <10 minutes, top 10 includes 8+ relevant
   - **Validation:** NDCG@10 >0.8

3. **Large Set Test (200 documents)**
   - **Input:** 200 emails, 25 urgent
   - **Test:** Scale test with parallel agents
   - **Expected:** Complete in <15 minutes, identify urgent items
   - **Validation:** Recall@25 >85%

4. **Custom Criteria Test**
   - **Input:** Same 50 documents, different criteria sets
   - **Test:** Run with meeting criteria, then paper criteria
   - **Expected:** Different rankings based on criteria
   - **Validation:** Results align with criteria definitions

5. **Edge Cases Test**
   - **Input:** Mix of very short and very long documents
   - **Test:** Handle varying document lengths
   - **Expected:** Fair assessment regardless of length
   - **Validation:** No bias toward long or short documents

6. **Quality Test**
   - **Setup:** Human experts rank same 100 documents
   - **Test:** Compare agent rankings to human rankings
   - **Expected:** High correlation (Spearman's rho >0.7)
   - **Validation:** Agent recommendations are trustworthy

### Performance Metrics

- **Ranking Quality:** NDCG, MAP, Precision@K, Recall@K
- **Speed:** Total processing time, per-document time
- **Scalability:** Speedup with parallel agents
- **Cost:** API costs per document
- **Agreement:** Inter-agent agreement rate

### Evaluation Methodology

1. **Ground Truth Creation:** Human experts label subset
2. **Ranking Comparison:** Compare agent vs. human rankings
3. **User Study:** Do users find recommendations useful?
4. **Time Savings:** How much time saved vs. manual review?

### Validation Criteria

- ✅ Process 100+ documents in <20 minutes
- ✅ Ranking quality NDCG@10 >0.75
- ✅ User satisfaction >80% in blind evaluation
- ✅ Cost <$0.10 per document processed
- ✅ Linear scalability (2x documents = 2x time, not 4x)

## Implementation Notes

### Architecture

```
Documents [D1, D2, ..., DN]
    ↓
Map Phase: N agents in parallel
├─ Agent 1 analyzes D1 → Vote + Score + Reasoning
├─ Agent 2 analyzes D2 → Vote + Score + Reasoning
├─ ...
└─ Agent N analyzes DN → Vote + Score + Reasoning
    ↓
Reduce Phase: Coordinator aggregates
├─ Collect all votes and scores
├─ Rank by priority
└─ Generate summary
    ↓
Output: Ranked list + Executive summary
```

### Configuration Example

```yaml
map_reduce_documents:
  map_phase:
    agents:
      count: 100  # One per document
      backend: gemini-2.0-flash  # Cost-effective
      pattern: parallel
      one_per_document: true
  
  reduce_phase:
    coordinator:
      backend: gpt-4o  # High-quality aggregation
      task: aggregate_and_rank
  
  criteria:
    type: meeting_notes
    requirements:
      - contains_action_items
      - contains_decisions
      - contains_conflicts
      - requires_follow_up
  
  output:
    top_n: 10
    include_summary: true
    include_reasoning: true
```

### Execution Command

```bash
# Meeting notes triage
massgen --config map_reduce_meetings.yaml \
  --documents ./meeting_notes/*.txt \
  --criteria action_items_and_decisions

# Paper selection
massgen --config map_reduce_papers.yaml \
  --documents ./papers/*.pdf \
  --criteria relevance_and_novelty \
  --top-n 15
```

## Related Work

- Multi-Agent Marketing Automation (Planned) - Similar map-reduce pattern
- Advanced Orchestration Patterns (Planned) - Parallel coordination
- Codebase Architecture Analysis (In Testing) - Parallel file reading

## References

**Key Innovation:** Shift from "which answer is better?" to "which item needs attention?" - enabling agents to filter signal from noise at scale for busy professionals.
