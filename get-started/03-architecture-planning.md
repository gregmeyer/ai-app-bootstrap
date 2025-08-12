# 03: Architecture & Planning

## Overview
This guide covers defining your system architecture, creating user stories, and planning your development approach.

## 🏗️ Architecture Definition

### 3.1 Core Architecture Decisions

Document key decisions in `agentContext/architecture/ddmmyyyy_architecture.md`:

**Example Architecture Decision Record:**
```markdown
# Architecture Decision - [Date]

## ADR-001: Technology Stack Choice
**Date:** [Date]  
**Status:** Confirmed

## Context
[What problem or situation led to this decision]

## Decision
[What was decided]

## Rationale
[Why this decision was made]

## Consequences
- **Positive:** [What this enables]
- **Negative:** [What this constrains]
- **Neutral:** [What this doesn't affect]

## Alternatives Considered
- [Alternative 1] - [Why rejected]
- [Alternative 2] - [Why rejected]

## Implementation Notes
[How to implement this decision]
```

### 3.2 Key Architecture Decisions to Document

- **ADR-001:** Technology stack choices
- **ADR-002:** Data flow architecture
- **ADR-003:** Communication patterns
- **ADR-004:** Security & authentication strategy
- **ADR-005:** Deployment strategy
- **ADR-006:** Testing approach

## 📖 User Stories & Tickets

### 3.3 Create User Stories

**Story Template (`stories/A-XX-template.md`):**
```markdown
# Story A-XX: [Story Title]

## Story Information
**Epic:** [Epic Name]  
**Priority:** [High/Medium/Low]  
**Status:** 🔴 Not Started  
**Technical Implementation:** T-XXX  

## User Story
**As a** [user type]  
**I want** [capability]  
**So that** [benefit/value]

## Acceptance Criteria
- [ ] [Specific, measurable criteria]
- [ ] [Another criteria]
- [ ] [Third criteria]

## Implementation Details
- **Technology:** [What technology to use]
- **Components:** [What parts are involved]
- **Integration:** [How it connects to other parts]

## Files to Create/Modify
- [Files needed for implementation]

## Dependencies
- [What needs to be built first]

## Notes
[Additional context, risks, or considerations]

## Related
- **Epic:** [Epic reference]
- **Ticket:** [T-XXX: Ticket Title]
- **Dependent Stories:** [What comes after this]
```

### 3.4 Epic Structure

**Epic A: Project Foundation**
- **Goal:** Establish the technical foundation
- **Stories:** A-01 through A-04

**Epic B: Core Functionality**
- **Goal:** Implement essential features
- **Stories:** B-01 through B-04

**Epic C: User Experience**
- **Goal:** Provide seamless user interaction
- **Stories:** C-01 through C-02

**Epic D: Advanced Features**
- **Goal:** Add sophisticated capabilities
- **Stories:** D-01 through D-02

### 3.5 Create Project Tickets

**Ticket Template (`tickets/T-XXX-template.md`):**
```markdown
# T-XXX: [Ticket Title]

## Ticket Information
**Epic:** [Epic Name]  
**Priority:** [High/Medium/Low]  
**Estimated Effort:** [X days]  
**Dependencies:** [T-XXX, T-XXX]  
**Status:** 🔴 Not Started  

## Description
[Clear description of what needs to be implemented]

## Acceptance Criteria
- [ ] [Specific, measurable criteria]
- [ ] [Another criteria]
- [ ] [Third criteria]

## Implementation Details
- **Technology:** [What technology to use]
- **Approach:** [How to implement it]
- **Considerations:** [Important factors]

## Files to Create/Modify
- [Files needed for implementation]

## Dependencies
- [What needs to be done first]

## Notes
[Additional context, risks, or considerations]

## Related
- **Story:** [Story reference]
- **Epic:** [Epic reference]
- **Next Tickets:** [What comes after this]
```

## 🚨 Gateway Stories

### 3.6 Critical Validation Points

**Gateway Story Template:**
```markdown
# Story A-XX: [Concept Name] Validation ⚠️ CRITICAL

## Story Information
**Epic:** A - Project Foundation  
**Priority:** Critical  
**Status:** 🔴 Not Started  
**Technical Implementation:** T-XXX  

## User Story
**As a** developer and stakeholder  
**I want** to validate that [concept] works in practice  
**So that** we can confidently continue building the full system

## Acceptance Criteria
- [ ] [Specific validation criteria 1]
- [ ] [Specific validation criteria 2]
- [ ] [Specific validation criteria 3]
- [ ] [Performance criteria]
- [ ] [User experience criteria]

## Implementation Details
- **Scope:** [What to validate]
- **Focus:** [What aspects are critical]
- **Testing:** [How to test it]
- **Success Criteria:** [What constitutes success]

## Files to Create/Modify
- [Files needed for validation]

## Dependencies
- [What needs to be built first]

## Notes
- This is a **gateway story** - if this fails, we need to reconsider the approach
- Should be implemented before committing to the full system
- Focus on proving the concept, not building production features

## Success Metrics
- **Technical:** [What must work technically]
- **User Experience:** [What must feel right]
- **Quality:** [What quality level is acceptable]
- **Performance:** [What performance is required]

## Risk Assessment
- **High Risk:** [What could completely fail]
- **Medium Risk:** [What could be problematic]
- **Low Risk:** [What's easily fixable]

## Go/No-Go Decision
This story serves as a **go/no-go decision point** for the entire project. If we cannot successfully:
1. [Critical requirement 1]
2. [Critical requirement 2]
3. [Critical requirement 3]

Then we should reconsider the project approach or pivot to a different solution.
```

### 3.7 Validation Framework

**Validation Criteria Template:**
```markdown
## Validation Framework

### Technical Validation
- [ ] Core functionality works
- [ ] Performance meets requirements
- [ ] Error handling works correctly
- [ ] Integration points function

### User Experience Validation
- [ ] Workflow feels natural
- [ ] Response times are acceptable
- [ ] Error messages are helpful
- [ ] Interface is intuitive

### Quality Validation
- [ ] 80%+ of basic use cases work
- [ ] Edge cases handled gracefully
- [ ] Data integrity maintained
- [ ] Security requirements met

### Performance Validation
- [ ] Response time < [X] seconds
- [ ] Throughput meets requirements
- [ ] Resource usage acceptable
- [ ] Scalability demonstrated
```

## 📋 Planning Process

### 3.8 Story Mapping Session

1. **Identify User Types**
   - Who will use this system?
   - What are their primary goals?

2. **Define User Journeys**
   - What are the main workflows?
   - What are the critical paths?

3. **Prioritize Stories**
   - Must have (MVP)
   - Should have (important)
   - Could have (nice to have)
   - Won't have (future)

4. **Identify Dependencies**
   - What must be built first?
   - What can be built in parallel?

### 3.9 Ticket Breakdown

1. **Break Stories into Tickets**
   - Each story becomes 1-3 tickets
   - Tickets should be 1-3 days of work
   - Clear acceptance criteria

2. **Estimate Effort**
   - Use story points or days
   - Consider complexity and uncertainty
   - Add buffer for unknowns

3. **Set Dependencies**
   - Technical dependencies
   - Resource dependencies
   - Business dependencies

## ✅ Verification Checklist

- [ ] Architecture decisions documented
- [ ] User stories created for all epics
- [ ] Project tickets defined
- [ ] Gateway stories identified
- [ ] Dependencies mapped
- [ ] Effort estimated
- [ ] Priorities set

## 🚀 Next Steps

After completing architecture and planning:

1. **[Environment & Configuration](04-environment-configuration.md)** - Set up your development environment
2. **[Backend Foundation](05-backend-foundation.md)** - Create your core service
3. **[Validation Framework](06-validation-framework.md)** - Prove your concept works

## 🔧 Troubleshooting

### Common Planning Issues

#### **Stories Too Large**
- Break down into smaller stories
- Focus on single user value
- Limit to 1-3 acceptance criteria

#### **Dependencies Unclear**
- Map technical dependencies
- Identify resource constraints
- Consider business requirements

#### **Priorities Unclear**
- Use MoSCoW method (Must, Should, Could, Won't)
- Get stakeholder input
- Consider business value

---

**Previous:** [CLI Implementation](02-cli-implementation.md) ← | **Next:** [Environment & Configuration](04-environment-configuration.md) →
