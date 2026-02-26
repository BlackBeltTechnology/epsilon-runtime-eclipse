# CI Flow — Development Versions and Branch Handling

This document describes the branching strategy, version numbering, and CI/CD pipeline for the epsilon-runtime-eclipse project.

## Branching Strategy

The project uses **GitFlow** ([reference](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)) with the following branch types:

| Branch | Purpose | Base |
|--------|---------|------|
| `develop` | Latest development sources of the active version | — |
| `feature/JNG-NUMBER_short_summary` | New feature development | `develop` |
| `release/*` (e.g. `1_0_beta1`) | Release stabilization and testing | `develop` |
| `bugfix/JNG-NUMBER_short_summary` | Fixes applied to release branches during testing | `release/*` |
| `support/JNG-NUMBER_short_summary` | Minor changes for a previous release | `release/*` |
| `master` | Latest released (stable) sources | — |
| `hotfix/JNG-NUMBER_short_summary` | Emergency fixes applied to `master` | `master` |

### Branch Flow

```mermaid
gitGraph
    commit id: "initial"
    branch develop
    commit id: "dev-1"
    branch feature/JNG-1
    commit id: "feat-1"
    commit id: "feat-2"
    checkout develop
    merge feature/JNG-1 id: "merge-feat"
    commit id: "dev-2"
    branch release/1.0
    commit id: "rc-1"
    branch bugfix/JNG-4
    commit id: "fix-1"
    checkout release/1.0
    merge bugfix/JNG-4 id: "merge-fix"
    checkout develop
    merge release/1.0 id: "merge-rel-to-dev"
    checkout main
    merge release/1.0 id: "release-1.0"
```

## Version Numbers

Version numbers follow **semantic versioning** with these rules:

| Event | Version Action |
|-------|---------------|
| Start a `feature/` branch | No version change (inherits from `develop`) |
| Start a `release/` branch | Increment 2nd number on `develop` |
| Start a `bugfix/` branch | No version change (applied on release branch) |
| Start a `support/` branch | Increment 3rd number |
| Start a `hotfix/` branch | Increment 4th number |

### CI Version Calculation

The CI pipeline calculates versions dynamically depending on the branch:

```mermaid
flowchart TD
    PUSH["Push / PR"] --> CHECK{Branch type?}
    CHECK -->|"master, release/*"| STABLE["Version from pom.xml\n(without -SNAPSHOT)"]
    CHECK -->|"develop, increment/*"| SNAPSHOT["major.minor.qualifier\n.YYYYMMDD_HHMMSS_commitId_branchName"]
    STABLE --> TAG["Git tag: v<version>"]
    SNAPSHOT --> TAG
```

## GitHub Actions Workflows

### build.yml — Main Build Pipeline

**Triggers:** Push to `develop`, PRs targeting `develop`, `master`, `release/*`, or `increment/*`.

```mermaid
flowchart TD
    TRIGGER["Push on develop\nor PR on develop/master/release/increment"] --> VERSION{Branch?}
    VERSION -->|"master, release/*"| STABLE_VER["Set version from pom.xml"]
    VERSION -->|"develop, increment/*"| SNAP_VER["Set version with timestamp + commit"]
    STABLE_VER --> BUILD["Build & deploy to Nexus"]
    SNAP_VER --> BUILD
    BUILD --> GIT_TAG["Create git tag v<version>"]
    GIT_TAG --> CHECK_PR{PR on release/increment?}
    CHECK_PR -->|Yes| MERGE_TAG["Create merge-pr/<version> tag"]
    MERGE_TAG --> TRIGGER_MERGE["Trigger merge-pr-tagged.yml"]
    CHECK_PR -->|No| CHECK_DEV{Push to develop?}
    CHECK_DEV -->|Yes| CHANGELOG["Build changelog\nCreate GitHub pre-release"]
    CHECK_DEV -->|No| DONE["Done"]
```

**Build steps:**
1. Checkout (depth 2) with JDK 21 (Zulu)
2. Configure Maven settings with `judong-nexus` mirror
3. Calculate version dynamically
4. Build and deploy artifacts to `nexus.judo.technology`
5. Upload P2 repository to `p2-judong` Nexus repo
6. Create GitHub release with changelog (develop only)

### merge-pr-tagged.yml — Auto-merge Release PRs

**Trigger:** Push of a `merge-pr/*` tag.

```mermaid
flowchart TD
    TAG["merge-pr/<version> tag pushed"] --> FORMAT{Version format?}
    FORMAT -->|"major.minor.qualifier\n(3 parts)"| MERGE_MASTER["Merge PR to master"]
    FORMAT -->|"Other"| SQUASH_DEV["Squash PR to develop"]
    MERGE_MASTER --> TRIGGER_RELEASE["Trigger create-release-on-master.yml"]
    SQUASH_DEV --> TRIGGER_BUILD["Trigger build.yml"]
    TRIGGER_RELEASE --> CLEANUP["Delete merge-pr/<version> tag"]
    TRIGGER_BUILD --> CLEANUP
```

### release.yml — Release Orchestration

**Trigger:** Manual dispatch with a version parameter (`auto` or explicit `major.minor.qualifier`).

```mermaid
flowchart TD
    MANUAL["Manual trigger\nwith version param"] --> CALC{Version = 'auto'?}
    CALC -->|Yes| FROM_POM["Release version from pom.xml\n(strip -SNAPSHOT)"]
    CALC -->|No| EXPLICIT["Use given version"]
    FROM_POM --> NEXT["Next version = qualifier + 1"]
    EXPLICIT --> NEXT
    NEXT --> PR_MASTER["Create PR on master\nwith release version"]
    NEXT --> PR_DEV["Create PR on develop\nwith next version"]
    PR_MASTER --> BUILD1["Trigger build.yml"]
    PR_DEV --> BUILD2["Trigger build.yml"]
```

### create-release-on-master.yml

**Trigger:** Push to `master`. Builds a changelog and creates a GitHub release (marked as latest).

### Other Workflows

| Workflow | Purpose | Trigger |
|----------|---------|---------|
| `bump-version.yml` | Bump version numbers | Manual dispatch |
| `build-dependabot.yml` | Build Dependabot PRs | Dependabot PRs |
| `jira-description-to-pr.yml` | Sync JIRA description to PR body | Manual trigger |
| `delete-old-draft-releases.yml` | Clean up stale draft releases | Scheduled |
| `sync-labels.yml` | Sync issue labels | Scheduled |

## Development Rules

> **Important:** There is no commit without a ticket number. Every commit and PR must include `JNG-xxx` referencing a [JIRA](https://blackbelt.atlassian.net/jira/) ticket.
