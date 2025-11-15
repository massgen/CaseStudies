# Web Agent Browsing

**Status:** 📋 Planned  
**Version:** Future  
**Last Updated:** November 15, 2025

## Overview

Agents autonomously browse and interact with web applications using Gemini 2.5 Computer Use and OpenAI Operator for complex web tasks, targeting benchmark performance on the Online Mind2Web Leaderboard.

## Description

### Goal
Enable agents to navigate real websites, fill forms, click buttons, and complete multi-step web tasks autonomously, competing with state-of-the-art web agents on standardized benchmarks.

### Key Features

1. **Visual Web Understanding**
   - Screenshot analysis to understand page layout
   - Element detection (buttons, forms, links, menus)
   - Context understanding (what page is this, what can I do)
   - Dynamic content handling (JavaScript-rendered pages)

2. **Intelligent Interaction**
   - Click appropriate elements
   - Fill forms with correct information
   - Navigate multi-page workflows
   - Handle popups, modals, cookie banners
   - Scroll and search for hidden elements

3. **Task Planning & Execution**
   - Decompose complex tasks into steps
   - Maintain context across page navigations
   - Recover from errors and dead ends
   - Verify task completion

4. **Multi-Modal Integration**
   - Process images, videos on web pages
   - Handle file uploads/downloads
   - Extract structured data from tables, lists
   - Interact with interactive widgets

5. **Benchmark Performance**
   - Target: Online Mind2Web Leaderboard
   - Metrics: Task success rate, action efficiency
   - Compare to: GPT-4V, Claude Computer Use, Gemini

### Example Tasks

- **E-commerce:** Search product, add to cart, checkout
- **Travel Booking:** Find flights, select dates, book tickets
- **Form Submission:** Fill multi-page survey, upload documents
- **Data Extraction:** Scrape information from multiple pages
- **Account Management:** Login, navigate settings, update profile

## Testing Guidelines

### Test Scenarios

1. **Simple Navigation Test**
   - **Task:** "Go to Wikipedia and search for 'Artificial Intelligence'"
   - **Expected:** Opens Wikipedia, uses search, navigates to article
   - **Validation:** Correct page reached in <30 seconds

2. **Form Filling Test**
   - **Task:** "Fill out contact form with name, email, message"
   - **Expected:** All fields filled correctly, form submitted
   - **Validation:** Form submission successful, no validation errors

3. **Multi-Step Workflow Test**
   - **Task:** "Search for 'laptop' on Amazon, filter by price under $1000, add cheapest to cart"
   - **Expected:** Completes all steps correctly
   - **Validation:** Correct item in cart at end

4. **Error Recovery Test**
   - **Task:** Intentionally provide invalid input (e.g., bad email format)
   - **Expected:** Agent recognizes error message, corrects input
   - **Validation:** Successfully recovers and completes task

5. **Complex Interaction Test**
   - **Task:** "Book a flight from NYC to SF on travel site"
   - **Expected:** Handles date picker, dropdowns, popups
   - **Validation:** Booking reaches confirmation page

6. **Mind2Web Benchmark Test**
   - **Setup:** Standard Mind2Web test suite
   - **Test:** Run agent on benchmark tasks
   - **Expected:** Success rate competitive with leaderboard
   - **Validation:** Submit results to leaderboard

### Benchmark Metrics

- **Success Rate:** Percentage of tasks completed correctly
- **Action Efficiency:** Number of actions to complete task
- **Time Efficiency:** Time to complete task
- **Error Rate:** Failed actions / total actions
- **Recovery Rate:** Successful recoveries / errors encountered

### Evaluation Methodology

1. **Functional Correctness:** Did the task complete successfully?
2. **Efficiency:** Minimal number of actions required?
3. **Robustness:** Handles edge cases and errors gracefully?
4. **Generalization:** Works across different websites?

### Validation Criteria

- ✅ >60% success rate on Mind2Web benchmark
- ✅ Average action efficiency within 1.5x of human baseline
- ✅ Handles 90%+ of common web UI patterns
- ✅ Error recovery rate >70%
- ✅ Leaderboard placement in top 10

## Implementation Notes

### Technology Stack

**Computer Use Tools:**
- Gemini 2.5 Computer Use (browser environment)
- OpenAI Operator (web agent capabilities)
- Claude Computer Use (browser-specific actions)

**Browser Automation:**
- Playwright for reliable browser control
- Chrome DevTools Protocol for advanced features
- Headless/headed mode support

**Visual Understanding:**
- Screenshot analysis with vision models
- Element detection and classification
- Layout understanding

### Configuration Example

```yaml
web_browsing:
  agent:
    backend: gemini-2.5-computer-use-preview
    environment: browser
    browser_type: chromium
    headless: false
  
  capabilities:
    screenshot_analysis: true
    element_detection: true
    multi_page_navigation: true
    form_filling: true
    error_recovery: true
  
  benchmarks:
    - mind2web
    - webarena
```

### Execution Command

```bash
# Single task
DISPLAY=:20 massgen --config web_browsing.yaml \
  --query "Search for 'machine learning' on Google Scholar"

# Benchmark evaluation
massgen --config web_browsing_benchmark.yaml \
  --benchmark mind2web \
  --output results.json
```

## Related Work

- Computer Use Tools (v0.1.9, v0.1.12) - Foundation for web browsing
- Gemini Computer Use (v0.1.12) - Browser automation capabilities
- Claude Computer Use - Browser environment support

## References

- [Online Mind2Web Leaderboard](https://huggingface.co/spaces/osunlp/Online_Mind2Web_Leaderboard)
- [Mind2Web Dataset](https://osu-nlp-group.github.io/Mind2Web/)
- [WebArena Benchmark](https://webarena.dev/)

## Target Benchmark

Compete on the Online Mind2Web Leaderboard, demonstrating MassGen's ability to handle real-world web automation tasks with state-of-the-art performance.
