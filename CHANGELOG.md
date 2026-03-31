# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.2.0] - 2026-03-31

### Fixed
- **Stage 2 accurate labeling**: Clarified that Stage 2 uses `prompt-auditor` Skill for prompt validation, not built-in engine
- **Dependencies documentation**: Updated prerequisite skills from 3 to 4 dependencies, explicitly listing `prompt-auditor`
- **EXPERIENCE.md visibility**: Added comprehensive documentation about EXPERIENCE.md file generation and role

### Added
- **Output artifacts documentation**: New "Pipeline Output" section explaining generated SKILL package structure
- **EXPERIENCE.md context**: Detailed explanation that Stage 3 auto-generates first part (3-5 structural experiences), with second part added during Stage 5
- **Skill Factory structure**: Complete directory layout showing all five stage implementations

### Changed
- README flow diagram: Stage 2 now clearly labeled as "prompt-auditor (Prompt Validation)"
- README prerequisites: Expanded from 3 to 4 required skills with full descriptions
- Documentation clarity: Enhanced accuracy for users creating new Skills

## [1.1.0] - 2026-03-28

### Added
- **Complete five-stage implementation**: Full directory structure for stages 0-5
- **EXPERIENCE.md auto-generation**: Stage 3 now generates first part of EXPERIENCE.md (3-5 structural experiences)
- **Skill output structure**: Includes SKILL.md, EXPERIENCE.md, scripts/, and references/
- **Comprehensive README**: Detailed documentation with flow diagrams and practical examples

### Fixed
- **Frontend/Backend Skill separation**: Independent implementations for frontend-coding-assistant and backend-coding-assistant
- **Skill dependency resolution**: Proper ordering and configuration of Stage-wise dependencies

## [1.0.0] - 2026-03-17

### Initial Release
- **Five-stage pipeline**: Complete workflow from requirements to audited SKILL package
- **Four core dependencies**: 
  - prompt-generator (Stage 1)
  - prompt-auditor (Stage 2)
  - skill-creator (Stage 3)
  - skill-auditor (Stage 4)
- **GitHub Actions CI**: Automated SKILL structure validation
- **Comprehensive documentation**: Architecture guides, best practices, problem classification examples
