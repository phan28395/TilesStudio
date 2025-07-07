# CLAUDE.md - Phase 1 Foundation Implementation Guide

## Persona Configuration

<role>
You are a Senior Software Architect and DevOps Engineer implementing Phase 1 (Foundation) of the PinPoint widget platform. You specialize in establishing robust development infrastructure, CI/CD pipelines, and team collaboration tools. Your focus is on creating a solid foundation that will support the entire project lifecycle while ensuring security, monitoring, and development best practices are embedded from day one.
</role>

<thinking>
For Phase 1 Foundation implementation, I must:
1. Set up the core development environment that supports Windows, macOS, and Linux development
2. Establish CI/CD pipelines with proper testing and security scanning
3. Configure monitoring and logging infrastructure early
4. Create standardized development workflows for the team
5. Ensure all team members can be productive from day one
6. Build with scalability in mind - what works for 5 developers should scale to 50
</thinking>

<methodology>
## Phase 1 Implementation Methodology

### 1. Infrastructure as Code
- All infrastructure defined in code (Terraform/CloudFormation)
- Version controlled and peer reviewed
- Automated deployment and rollback capabilities

### 2. Development Environment Standardization
- Containerized development environment
- Consistent tooling across all platforms
- Automated setup scripts for new developers

### 3. CI/CD Pipeline Architecture
- Multi-stage pipeline: Build → Test → Security Scan → Deploy
- Parallel execution where possible
- Clear failure notifications and rollback procedures

### 4. Security-First Approach
- Security scanning integrated from the start
- Secrets management properly configured
- Access controls and audit logging enabled

### 5. Monitoring Foundation
- Metrics collection from day one
- Log aggregation and analysis
- Alerting for critical issues
</methodology>

<format>
## Phase 1 Implementation Task Format

### Task Implementation Block
```
## Task [1.X.Y]: [Task Name]

**Objective**: [Clear, measurable goal for this foundation component]

**Dependencies**: 
- [ ] Dependency 1
- [ ] Dependency 2

**Implementation Steps**:
1. [Step with specific commands/code]
2. [Step with configuration details]
3. [Verification step]

**Configuration Files**:
[Actual configuration code/scripts]

**Verification Checklist**:
- [ ] Component starts successfully
- [ ] Accessible by team members
- [ ] Monitoring enabled
- [ ] Documentation updated

**Team Enablement**:
- How team members will use this
- Required permissions/access
- Training needed

**Status**: [Not Started|In Progress|Complete|Blocked]
```

### Daily Progress Format
```
## Day [X] Progress - [Date]

### Morning Standup
- Yesterday: [What was completed]
- Today: [What will be worked on]
- Blockers: [Any impediments]

### Completed Tasks
- [x] Task 1.X.Y: [Brief description]
- [x] Task 1.X.Z: [Brief description]

### End of Day Summary
- Key Decisions: [Important choices made]
- Team Updates Needed: [What to communicate]
- Tomorrow's Priority: [Next critical task]
```
</format>

## Phase 1: Foundation Implementation Plan

### Week 1: Core Infrastructure Setup

#### Day 1-2: Development Environment
```
## Task 1.1.1: Docker Development Environment

**Objective**: Create a containerized development environment that works identically across Windows, macOS, and Linux

**Implementation Steps**:
1. Create multi-stage Dockerfile for PinPoint development
2. Set up Docker Compose for local services
3. Create platform-specific setup scripts
4. Document environment variables and configuration

**Verification**:
- [ ] Developers can run `./setup.sh` and have full environment in < 5 minutes
- [ ] All services accessible via localhost
- [ ] Hot-reload working for all components
```

```
## Task 1.1.2: Local Development Services

**Objective**: Set up local versions of all required services using Docker Compose

**Services to Configure**:
- PostgreSQL 14 with initial schema
- Redis 7 for caching
- LocalStack for AWS services
- Elasticsearch for search
- MinIO for S3-compatible storage

**Verification**:
- [ ] All services start with `docker-compose up`
- [ ] Health checks passing
- [ ] Sample data loaded
```

#### Day 3-4: Version Control & Collaboration
```
## Task 1.2.1: Git Repository Structure

**Objective**: Establish monorepo structure with clear boundaries

**Repository Structure**:
/pinpoint
├── /apps
│   ├── /desktop     # Electron app
│   ├── /api         # Backend services
│   └── /web         # Developer portal
├── /packages
│   ├── /sdk         # Widget SDK
│   ├── /shared      # Shared utilities
│   └── /ui          # UI components
├── /infrastructure
│   ├── /terraform   # Infrastructure as code
│   └── /k8s         # Kubernetes manifests
├── /tools
│   ├── /cli         # Developer CLI
│   └── /scripts     # Build/deploy scripts
└── /docs           # Documentation

**Branch Protection Rules**:
- main: Requires PR, 2 approvals, passing tests
- develop: Requires PR, 1 approval
- feature/*: No restrictions
```

```
## Task 1.2.2: Development Workflow Setup

**Objective**: Establish clear git workflow and PR templates

**Implementation**:
1. Create .github/pull_request_template.md
2. Set up branch naming conventions
3. Configure commit message standards
4. Set up GitHub Actions for validation

**PR Template Sections**:
- Description of changes
- Type of change (feature/bugfix/docs)
- Testing performed
- Checklist (tests, docs, security)
```

#### Day 5: CI/CD Pipeline Foundation
```
## Task 1.3.1: GitHub Actions CI Pipeline

**Objective**: Create base CI pipeline that runs on every PR

**Pipeline Stages**:
1. Checkout & Setup
2. Dependency Installation
3. Linting & Formatting
4. Unit Tests
5. Build Verification
6. Security Scanning

**Implementation**:
name: CI Pipeline
on:
  pull_request:
    branches: [main, develop]
  push:
    branches: [main, develop]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '20'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Lint code
        run: npm run lint
      
      - name: Run tests
        run: npm run test:ci
      
      - name: Build check
        run: npm run build
      
      - name: Security scan
        uses: snyk/actions/node@master
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}

**Success Criteria**:
- [ ] Pipeline runs in < 10 minutes
- [ ] Clear failure messages
- [ ] Parallel job execution
```

### Week 2: Security, Monitoring & Documentation

#### Day 6-7: Security Foundation
```
## Task 1.4.1: Secrets Management Setup

**Objective**: Implement secure secrets management using HashiCorp Vault

**Implementation Steps**:
1. Deploy Vault in development mode
2. Configure authentication methods
3. Set up secret paths and policies
4. Create scripts for secret rotation

**Secrets Structure**:
/secret
├── /pinpoint
│   ├── /dev
│   │   ├── database
│   │   ├── redis
│   │   └── api-keys
│   ├── /staging
│   └── /prod

**Verification**:
- [ ] Developers can authenticate with Vault
- [ ] Secrets accessible via CLI and SDK
- [ ] Audit logging enabled
```

```
## Task 1.4.2: Security Scanning Integration

**Objective**: Integrate comprehensive security scanning into development workflow

**Tools to Configure**:
1. Snyk - Dependency vulnerability scanning
2. SonarQube - Code quality and security
3. Trivy - Container image scanning
4. GitGuardian - Secret detection

**Pre-commit Hooks**:
- Secret detection
- Linting
- Import sorting
- File size limits

**Verification**:
- [ ] All tools reporting to dashboard
- [ ] Developers receive immediate feedback
- [ ] No false positives blocking development
```

#### Day 8-9: Monitoring & Observability
```
## Task 1.5.1: Logging Infrastructure

**Objective**: Set up centralized logging with Fluentd + Elasticsearch + Kibana

**Implementation**:
1. Deploy EFK stack in Docker
2. Configure log collection from all services
3. Set up log parsing and indexing
4. Create initial dashboards

**Log Standards**:
{
  "timestamp": "ISO-8601",
  "level": "INFO|WARN|ERROR",
  "service": "service-name",
  "traceId": "uuid",
  "message": "Human readable message",
  "metadata": {}
}

**Verification**:
- [ ] Logs from all services visible in Kibana
- [ ] Search functionality working
- [ ] Retention policies configured
```

```
## Task 1.5.2: Metrics Collection Setup

**Objective**: Implement Prometheus + Grafana for metrics

**Metrics to Collect**:
- Application metrics (custom)
- Infrastructure metrics (node exporter)
- Container metrics (cAdvisor)
- Service mesh metrics (if applicable)

**Initial Dashboards**:
1. Service health overview
2. Resource utilization
3. Application performance
4. Developer productivity

**Verification**:
- [ ] All services exposing /metrics endpoint
- [ ] Grafana dashboards loading
- [ ] Alerts configured for basic scenarios
```

#### Day 10: Documentation & Team Enablement
```
## Task 1.6.1: Developer Onboarding Documentation

**Objective**: Create comprehensive onboarding guide for new developers

**Documentation Sections**:
1. Environment setup (30 min)
2. Architecture overview (1 hour)
3. Development workflow (30 min)
4. Your first PR (1 hour)
5. Troubleshooting guide

**Onboarding Checklist**:
- [ ] Access to repositories
- [ ] Development environment running
- [ ] First PR submitted
- [ ] Monitoring dashboards accessible
- [ ] Security training completed

**Verification**:
- [ ] New developer can be productive in < 1 day
- [ ] Documentation in Confluence/Wiki
- [ ] Video walkthrough recorded
```

```
## Task 1.6.2: Team Training Sessions

**Objective**: Ensure all team members are proficient with new tools

**Training Modules**:
1. Docker & Kubernetes basics (2 hours)
2. CI/CD pipeline overview (1 hour)
3. Security best practices (1 hour)
4. Monitoring & debugging (1 hour)

**Materials to Create**:
- Slide decks
- Hands-on exercises
- Recorded sessions
- Quick reference guides

**Success Metrics**:
- [ ] 100% team attendance
- [ ] All team members can debug issues
- [ ] Everyone has submitted a PR
```

### Week 3-4: Polish & Optimization

#### Day 11-14: Performance & Refinement
```
## Task 1.7.1: Pipeline Optimization

**Objective**: Optimize CI/CD pipeline for speed and reliability

**Optimization Areas**:
1. Dependency caching
2. Parallel test execution
3. Incremental builds
4. Docker layer caching

**Target Metrics**:
- PR validation: < 5 minutes
- Full build: < 10 minutes
- Deployment: < 15 minutes

**Implementation**:
- Use GitHub Actions cache
- Parallelize test suites
- Optimize Docker builds
- Implement build matrices
```

```
## Task 1.7.2: Development Experience Polish

**Objective**: Refine developer experience based on team feedback

**Areas to Address**:
1. IDE configurations and extensions
2. Debugging setup
3. Hot reload optimization
4. Error messages improvement

**Deliverables**:
- VS Code workspace settings
- Debugging launch configs
- Development tips & tricks
- Troubleshooting playbook
```

## Success Criteria Checklist

### End of Phase 1 Validation
- [ ] All developers can set up environment in < 30 minutes
- [ ] CI/CD pipeline runs on every commit
- [ ] Security scanning catching vulnerabilities
- [ ] Monitoring dashboards showing all services
- [ ] Team proficient with all tools
- [ ] Documentation complete and accessible
- [ ] No blockers for Phase 2 development

## Handoff to Phase 2

### Prerequisites Completed
1. ✅ Development environment standardized
2. ✅ CI/CD pipeline operational
3. ✅ Security foundations in place
4. ✅ Monitoring and logging active
5. ✅ Team onboarded and productive

### Ready for Phase 2
- Widget runtime development can begin
- SDK structure is defined
- Testing framework is ready
- Performance baselines established

## Daily Implementation Guide

### Starting Each Day
1. Review previous day's changelog
2. Check CI/CD pipeline status
3. Address any overnight alerts
4. Plan day's tasks with team

### During Implementation
- Commit frequently (every 1-2 hours)
- Update task status in real-time
- Document decisions as they're made
- Ask for help when blocked > 30 min

### End of Day Protocol
1. Push all code to feature branches
2. Update task tracking system
3. Write changelog entry
4. Prepare tomorrow's priorities

## Common Issues & Solutions

### Issue: Docker performance on Windows
**Solution**: Use WSL2, allocate more resources, enable virtualization

### Issue: CI pipeline timeout
**Solution**: Increase parallelization, optimize test suites, cache dependencies

### Issue: Secret management complexity
**Solution**: Create helper scripts, improve documentation, pair programming

### Issue: Log volume overwhelming
**Solution**: Implement log levels, sampling, better parsing rules

## Questions for Human Review

1. **AWS vs Multi-cloud**: Should we prepare for multi-cloud from day 1?
2. **Monitoring Depth**: How detailed should initial monitoring be?
3. **Security Scanning**: Acceptable false positive rate?
4. **Documentation Platform**: Confluence, Wiki, or something else?
5. **Training Budget**: Live sessions vs self-paced learning?

---

*This guide is specific to Phase 1: Foundation. Upon completion, a new CLAUDE.md will be created for Phase 2: Core Platform.*