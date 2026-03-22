# Family Wallet - AI-Augmented Product Management

A complete project structure for managing Family Wallet product development using AI-assisted workflows.

## Structure
```
FamilyWallet-claude-main/
├── CLAUDE.md                     ← Project brain (overview, modules, goals)
├── AGENTS.md                     ← AI rules and validation
├── .claude/
│   ├── commands/
│   │   └── analyse-transcript.md ← M4: Analyze meeting transcripts
│   └── skills/
│       ├── transcript-analysis-framework.md    ← M4: How to analyze
│       └── user-story-enrichment-framework.md  ← M4: How to enrich stories
├── domain/
│   └── dictionary.md             ← Domain definitions, risks, decisions
├── modules/
│   └── financial_consultation/   ← Product module (Financial Consultation feature)
└── tactics/
    ├── backlog/
    │   └── financial_consultation_user_stories.md ← Initial user stories
    └── refinements/
        └── summary.md            ← Meeting summary (Nov 15, 2025)
```

## Module 4: Transcript Analysis Workflow

### Input
- Meeting transcript (raw or structured markdown)
- Existing user stories in @tactics/backlog/
- Project context in @CLAUDE.md and @domain/dictionary.md

### Process
1. Run `/analyse-transcript` command
2. Skill activates: transcript-analysis-framework
3. Output: Summary with Process Points, Risks, Questions
4. Optional: Enrich user stories with transcript insights

### Output
- Enhanced user stories with new AC, risks, technical constraints
- Diff review for approval before saving

## Next Steps

1. Test `/analyse-transcript` command
2. Enrich financial_consultation_user_stories.md with meeting insights
3. Review diffs and approve changes
4. Prepare for Module 5 (Knowledge graphs with Obsidian)
