# MassGen Video Recording and Editing

**Status:** 📋 Planned  
**Version:** Future  
**Last Updated:** November 15, 2025

## Overview

Auto-generate case study videos by running commands with recording, editing (speed up, add captions, highlight logs), cutting unnecessary parts, and producing 1-minute demo videos automatically for showcasing MassGen capabilities.

## Description

### Goal
Automate the entire video production pipeline for MassGen case studies: from running the command to publishing a polished 1-minute demo video, eliminating manual video editing work.

### Key Features

1. **Automated Recording**
   - Record entire MassGen execution session
   - Capture terminal output, logs, and visual output
   - Track important events (agent responses, tool calls, results)
   - Support both terminal and browser recording

2. **Intelligent Editing**
   - Speed up boring sections (compilation, waiting, repetitive output)
   - Cut unnecessary parts (errors that were recovered, redundant logs)
   - Highlight key moments (final answers, important decisions, insights)
   - Add captions for important commands and outputs
   - Picture-in-picture for multi-agent coordination

3. **Content Analysis**
   - Video understanding to identify key frames
   - Log analysis to find important events
   - Automatic chapter markers
   - Generate video description and keywords

4. **Production Quality**
   - Add intro/outro sequences
   - Background music (optional)
   - Smooth transitions between sections
   - Professional color grading and effects
   - Export in optimal format and resolution

5. **Multi-Format Output**
   - Full recording (for documentation)
   - 1-minute highlight reel (for social media)
   - 30-second teaser (for Twitter)
   - GIF animations (for docs/GitHub)
   - Tutorial segments (for YouTube)

### Workflow

```
Run MassGen Command
    ↓
Record Session (asciinema/OBS)
    ↓
Analyze Recording (identify key moments)
    ↓
Edit Video (speed up, cut, add effects)
    ↓
Generate Multiple Formats
    ↓
Publish (YouTube, Twitter, Docs)
```

## Testing Guidelines

### Test Scenarios

1. **Short Task Recording (5 min execution)**
   - **Task:** Simple research query
   - **Test:** Record, edit, produce 1-min video
   - **Expected:** Captures key moments, smooth pacing
   - **Validation:** Video is watchable and informative

2. **Long Task Recording (30 min execution)**
   - **Task:** Complex multi-agent workflow
   - **Test:** Handle long recording, intelligent time-lapse
   - **Expected:** Condense to 2-3 minutes without losing narrative
   - **Validation:** All major steps visible, progression clear

3. **Multi-Agent Recording**
   - **Task:** Parallel agent execution with coordination
   - **Test:** Show multiple agents working simultaneously
   - **Expected:** Picture-in-picture or split-screen layout
   - **Validation:** Easy to follow, coordination visible

4. **Error Recovery Recording**
   - **Task:** Task with failure and recovery
   - **Test:** Show error briefly, then skip to recovery
   - **Expected:** Error visible but not dwelled on
   - **Validation:** Maintains flow, shows resilience

5. **Caption Accuracy Test**
   - **Task:** Recording with important commands/outputs
   - **Test:** Auto-generate captions for key moments
   - **Expected:** Captions are accurate, well-timed, readable
   - **Validation:** Human review of caption quality

6. **Full Pipeline Test**
   - **Task:** Run case study, produce video automatically
   - **Test:** End-to-end automation with no manual intervention
   - **Expected:** Publication-ready video in <10 minutes
   - **Validation:** Video quality suitable for public sharing

### Quality Metrics

**Technical Quality:**
- Resolution: 1080p minimum
- Frame rate: 30fps minimum
- Audio quality: Clear, no artifacts
- Compression: Balanced quality/size

**Content Quality:**
- Narrative clarity: Easy to follow
- Pacing: Not too fast or slow
- Information density: Key points visible
- Engagement: Holds attention

**Production Value:**
- Transitions: Smooth and professional
- Captions: Readable and well-placed
- Effects: Subtle and helpful
- Branding: Consistent with MassGen identity

### Validation Criteria

- ✅ Full automation: command → published video
- ✅ 10:1 compression ratio (10 min → 1 min) without losing key info
- ✅ Human evaluation: Video quality >7/10
- ✅ Caption accuracy >95%
- ✅ Processing time <10 minutes per video
- ✅ Videos suitable for public sharing (YouTube, Twitter)

## Implementation Notes

### Technical Requirements

**Recording:**
- asciinema for terminal (from v0.1.14)
- OBS Studio for screen capture
- Browser automation recording
- Event logging for synchronization

**Video Understanding:**
- Multimodal models (v0.1.3) for frame analysis
- Log parsing for event detection
- Importance scoring for key moments

**Editing:**
- FFmpeg for video manipulation
- Python libraries: moviepy, opencv
- Automated editing scripts
- Caption generation

**Planning Mode:**
- Break editing into phases
- Coordinate multiple tools
- Handle >10min processing time
- Progress tracking and reporting

### Configuration Example

```yaml
video_production:
  recording:
    mode: auto
    capture: screen_and_terminal
    fps: 30
    resolution: 1920x1080
  
  editing:
    speed_up_threshold: 5  # Speed up if no activity for 5s
    max_duration: 60  # Target 1 minute
    cut_errors: true
    add_captions: true
    highlight_key_moments: true
  
  output_formats:
    - full_recording  # Complete session
    - highlight_reel  # 1 minute
    - teaser  # 30 seconds
    - gif_animations  # Key moments
  
  production:
    intro_outro: true
    background_music: subtle
    branding: massgen
```

### Execution Command

```bash
# Record and auto-produce video
massgen --config case_study.yaml \
  --query "Research AI trends and write report" \
  --record \
  --auto-edit \
  --output-video ./demos/ai_trends.mp4

# Batch process multiple recordings
massgen-video-edit \
  --recordings ./recordings/*.cast \
  --template highlight_reel \
  --output ./videos/
```

## Related Work

- Terminal Evaluation (v0.1.14 planned) - Session recording with asciinema
- Multimodal Video Analysis (v0.1.3) - Video understanding
- Automation Mode (v0.1.8) - Structured output for analysis

## References

**Key Value:** Reduce video production time from hours to minutes, enabling rapid case study publication and increasing MassGen visibility through high-quality demo videos.

**Target Output:** Professional demo videos similar to existing MassGen case study videos on YouTube, but produced automatically.
