---
inclusion: always
---

# Changelog Maintenance

This steering document ensures comprehensive change tracking and documentation for project specifications and implementation.

## Change Documentation Requirements

### When to Update CHANGELOG.md

**ALWAYS update the changelog when making changes to:**
- Requirements specifications (functional or non-functional)
- Architecture decisions or technology stack changes
- Security policies or implementation approaches
- Cost models, performance targets, or scaling assumptions
- API interfaces, data models, or system boundaries
- Testing strategies, quality gates, or evaluation criteria
- Deployment configurations or environment setups
- Project scope, feature definitions, or acceptance criteria

### Changelog Entry Format

Each changelog entry MUST include:

```markdown
### [Change Type]: [Brief Description]
**Problem**: [What issue was being addressed]
**Solution**: [What was implemented/changed]
**Files Updated**: [List of modified files]
**Impact**: [Business/technical impact of the change]
```

### Change Types

Use these standardized change types:
- **CRITICAL FIX**: Must-fix issues that could cause project failure
- **ARCHITECTURE**: Major architectural decisions or technology changes
- **SECURITY**: Security enhancements, vulnerability fixes, or policy updates
- **SCOPE**: Scope clarifications, feature additions/removals, or boundary changes
- **QUALITY**: Testing, evaluation, or quality assurance improvements
- **CONSISTENCY**: Terminology fixes, documentation alignment, or cleanup
- **ENHANCEMENT**: Optional improvements or nice-to-have features

### Documentation Standards

**Be Specific**: 
- Include exact file names and section references
- Quantify changes where possible (e.g., "17 terminology fixes")
- Reference specific requirements or design sections affected

**Explain Impact**:
- Why the change was necessary
- What risks it mitigates or benefits it provides
- How it affects implementation or operations

**Maintain Traceability**:
- Link changes to original requirements or design decisions
- Reference related changes in other documents
- Note dependencies or follow-up actions required

## Change Review Process

### Before Making Changes
1. **Identify scope** of the proposed change
2. **Assess impact** on other specification documents
3. **Plan updates** across all affected files

### During Changes
1. **Update all affected documents** consistently
2. **Maintain terminology** alignment across specifications
3. **Verify internal consistency** of changes

### After Changes
1. **Update CHANGELOG.md** with comprehensive entry
2. **Run diagnostics** on all modified files
3. **Verify cross-document consistency**
4. **Update version numbers** if significant changes

## Quality Gates

### Changelog Completeness Check
Before considering any specification update complete:
- [ ] CHANGELOG.md updated with new entry
- [ ] All modified files listed in changelog
- [ ] Impact assessment documented
- [ ] Change rationale explained
- [ ] Related changes cross-referenced

### Cross-Document Consistency
Verify that changes maintain consistency across:
- [ ] Requirements ↔ Design alignment
- [ ] Design ↔ Tasks alignment  
- [ ] README ↔ Specifications alignment
- [ ] Terminology usage across all documents
- [ ] Architecture assumptions and dependencies

## Automation Reminders

When using Kiro for specification updates:
- **Always ask**: "Should I update the changelog for these changes?"
- **Cross-check**: Verify changes don't create inconsistencies in other documents
- **Version control**: Consider if changes warrant version number updates
- **Impact assessment**: Evaluate if changes affect implementation timeline or costs
- **Stakeholder communication**: Consider if changes require stakeholder notification

## Examples

### Good Changelog Entry
```markdown
### CRITICAL FIX: Glossary vs Requirements Naming Drift
**Problem**: Inconsistent terminology between `System_Component` and `Core_Agent` blurred system vs component responsibilities, affecting logging, testing, and future expansion.
**Solution**: Applied consistent naming rule globally - `Core_Agent` for business logic, `System_Component` for infrastructure/security.
**Files Updated**: requirements.md (17 terminology fixes)
**Impact**: Cleaner separation of concerns, improved testability, easier future implementation
```

### Poor Changelog Entry
```markdown
### Fixed naming issues
Updated some terminology in requirements.
```

## Maintenance Schedule

- **Weekly**: Review changelog for completeness during active development
- **Monthly**: Verify cross-document consistency and update version numbers
- **Pre-Release**: Comprehensive changelog review and impact assessment
- **Post-Release**: Archive completed changelog entries and start new version section
- **Milestone Reviews**: Assess changelog completeness at major project milestones

---

*This steering document ensures that all specification changes are properly documented, tracked, and communicated to maintain project transparency and traceability across any software development project.*