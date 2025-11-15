# Priority-Based Document Ranking

**Status:** 📋 Planned  
**Version:** Future  
**Last Updated:** November 15, 2025

## Overview

Vote on document importance for busy managers and researchers: meeting notes, conference papers, stock news, emails. Information-to-attention prediction with custom voting criteria to filter signal from noise at scale.

## Description

### Goal
Help busy professionals focus on what matters by intelligently ranking documents based on customizable importance criteria, shifting from "which answer is better?" to "which document deserves my attention?"

### Key Features

1. **User-Defined Importance Criteria**
   - **For Managers:** Impact on team, requires decisions, time-sensitive
   - **For Researchers:** Novel findings, relevant to current work, high-quality methods
   - **For Traders:** Market-moving information, actionable insights, time-critical
   - **For Executives:** Strategic implications, requires approval, high-stakes

2. **Intelligent Voting System**
   - Each agent votes on document importance
   - Multi-dimensional scoring (urgency, impact, relevance, quality)
   - Confidence-weighted aggregation
   - Explanation for each vote

3. **Priority Tiers**
   - **Urgent:** Requires immediate attention (today)
   - **High:** Important, review within 2-3 days
   - **Medium:** Valuable, review when available
   - **Low:** Optional, archive or skip
   - **Filter Out:** Not relevant, safe to ignore

4. **Context-Aware Ranking**
   - Consider user's role and responsibilities
   - Account for current projects and priorities
   - Learn from past attention patterns
   - Adapt to changing circumstances

5. **Actionable Summaries**
   - Executive summary of top-priority items
   - Key action items extracted
   - Suggested responses or next steps
   - Time estimates for review

### Use Cases

**Meeting Notes Prioritization:**
- Input: 50 meeting transcripts from past month
- Criteria: Contains decisions, action items requiring follow-up, conflicts
- Output: Top 10 meetings ranked by priority with action items

**Conference Paper Selection:**
- Input: 100 papers from latest conference proceedings
- Criteria: Relevance to research area, methodological novelty, result quality
- Output: Top 15 papers ranked for detailed reading

**Stock News Analysis:**
- Input: 500 news articles about market sectors
- Criteria: Material impact on holdings, actionable insights, time-sensitivity
- Output: Top 30 articles requiring attention for trading decisions

**Email Triage:**
- Input: 200 emails from past 10 days
- Criteria: Urgency, requires decision, high-impact, from key stakeholders
- Output: Top 20 emails prioritized with suggested responses

## Testing Guidelines

### Test Scenarios

1. **Meeting Notes Test (Manager)**
   - **Input:** 30 meeting notes, 5 with critical action items
   - **Criteria:** Action items, decisions, conflicts, team impact
   - **Expected:** Top 5 includes all critical meetings
   - **Validation:** Precision@5 = 100%, Recall@5 = 100%

2. **Paper Selection Test (Researcher)**
   - **Input:** 50 papers, 10 highly relevant to current research
   - **Criteria:** Relevance, novelty, methodology quality
   - **Expected:** Top 10 includes 8+ relevant papers
   - **Validation:** NDCG@10 >0.8

3. **Email Triage Test (Executive)**
   - **Input:** 100 emails, 15 requiring immediate response
   - **Criteria:** Urgency, importance, requires decision
   - **Expected:** Top 15 captures most urgent emails
   - **Validation:** Recall@15 >80%

4. **Stock News Test (Trader)**
   - **Input:** 200 market news articles, 20 with actionable insights
   - **Criteria:** Market impact, actionability, time-sensitivity
   - **Expected:** Top 20 contains market-moving information
   - **Validation:** Trader satisfaction >80%

5. **Custom Criteria Test**
   - **Input:** Same 50 documents with different criteria
   - **Test:** Run with manager criteria, then researcher criteria
   - **Expected:** Different rankings reflecting different priorities
   - **Validation:** Rankings align with specified criteria

6. **Learning Test**
   - **Setup:** Track user's actual attention over time
   - **Test:** Improve ranking based on historical patterns
   - **Expected:** Ranking accuracy improves with more data
   - **Validation:** 10% improvement in NDCG after 100 documents

### Evaluation Metrics

**Ranking Quality:**
- NDCG@K (Normalized Discounted Cumulative Gain)
- Precision@K and Recall@K
- Mean Average Precision (MAP)
- Spearman correlation with ideal ranking

**User Satisfaction:**
- % of top-ranked items that received attention
- % of important items successfully surfaced
- Time saved vs. manual review
- User confidence in rankings

**Efficiency:**
- Processing time per document
- Cost per ranking
- Scalability (documents processed per hour)

### Validation Methodology

1. **Ground Truth Creation:**
   - Users manually label subset of documents
   - Track actual attention patterns over time
   - Expert labeling for specialized domains

2. **A/B Testing:**
   - Random ranking vs. AI ranking
   - Measure which surfaces more important items
   - Track user satisfaction and time saved

3. **Longitudinal Study:**
   - Monitor ranking accuracy over months
   - Measure improvement from learning
   - Track user trust and adoption

### Validation Criteria

- ✅ Ranking quality NDCG@10 >0.75 across use cases
- ✅ User satisfaction >80% in blind evaluation
- ✅ Time savings >50% vs. manual review
- ✅ Learning improves accuracy by >10% over time
- ✅ Cost <$0.05 per document ranked

## Implementation Notes

### Architecture

```
Documents [D1, D2, ..., DN]
    ↓
User Profile + Current Context
    ↓
Parallel Voting (one agent per document)
├─ Agent 1: Score D1 on criteria → Vote + Confidence
├─ Agent 2: Score D2 on criteria → Vote + Confidence
├─ ...
└─ Agent N: Score DN on criteria → Vote + Confidence
    ↓
Aggregation & Ranking
    ├─ Weight by confidence
    ├─ Apply user preferences
    └─ Generate priority tiers
    ↓
Output: Ranked list + Executive summary
```

### Configuration Example

```yaml
priority_ranking:
  user_profile:
    role: engineering_manager
    responsibilities:
      - team_coordination
      - technical_decisions
      - stakeholder_communication
  
  criteria:
    - name: requires_action
      weight: 0.4
      description: Contains action items for manager
    
    - name: time_sensitive
      weight: 0.3
      description: Urgent, time-critical information
    
    - name: team_impact
      weight: 0.2
      description: Affects team operations or morale
    
    - name: strategic
      weight: 0.1
      description: Long-term strategic implications
  
  voting:
    agents_per_document: 1
    backend: gemini-2.0-flash  # Cost-effective
    confidence_weighting: true
  
  output:
    tiers: [urgent, high, medium, low, filter]
    include_summaries: true
    include_actions: true
```

### Execution Command

```bash
# Rank meeting notes
massgen --config priority_ranking_manager.yaml \
  --documents ./meetings/*.txt \
  --profile manager_profile.yaml

# Rank papers
massgen --config priority_ranking_researcher.yaml \
  --documents ./papers/*.pdf \
  --profile researcher_profile.yaml

# Rank emails
massgen --config priority_ranking_executive.yaml \
  --documents ./emails/*.eml \
  --profile executive_profile.yaml
```

### Output Format

```markdown
# Priority Ranking Report

## Urgent (Immediate Attention Required)
1. [Meeting 2024-11-14] Team Conflict Resolution
   - Priority Score: 9.5/10
   - Reason: Requires immediate decision on team restructuring
   - Action: Schedule follow-up by EOD

2. [Email from CEO] Q4 Strategy Approval
   - Priority Score: 9.2/10
   - Reason: Needs approval for budget allocation
   - Action: Review and respond today

## High Priority (Review Within 2-3 Days)
3. [Paper] Novel Approach to Scaling
   - Priority Score: 8.1/10
   - Reason: Directly relevant to current project
   - Action: Read and evaluate for implementation

[... continues ...]

## Executive Summary
- 2 urgent items requiring immediate attention
- 5 high-priority items for this week
- Estimated review time: 3.5 hours
```

## Related Work

- Map-Reduce Document Processing (Planned) - Same parallel voting pattern
- Multi-Agent Marketing Automation (Planned) - Scalability pattern
- Advanced Orchestration Patterns (Planned) - Parallel coordination

## References

**Key Innovation:** Context-aware, multi-dimensional priority ranking that adapts to user role, responsibilities, and historical attention patterns - not just generic "importance" but personalized relevance.

## Target Impact

Enable busy professionals to handle 10x more documents by focusing only on what truly matters, reducing information overload while ensuring critical items are never missed.
