# BRI HR Consultancy Portal - User Flow Diagrams

## 1. Candidate User Flow

```mermaid
flowchart TD
    Start([Candidate Visits Portal]) --> Signup[Sign Up / Login]
    Signup --> EmailVerif{Email Verification}
    EmailVerif -->|Verified| Profile[Complete Profile]
    EmailVerif -->|Not Verified| ResendEmail[Resend Verification Email]
    ResendEmail --> EmailVerif
    
    Profile --> BioData[Submit Bio-Data]
    BioData --> Experience[Enter Years of Experience]
    Experience --> Skills[Add Skills]
    Skills --> SelfRate[Self-Rate Skills 1-5]
    SelfRate --> Future1[Future: AI-Based Rating Adjustment]
    
    Future1 --> Portfolio[Submit Projects/Portfolio]
    Portfolio --> GithubLink[Add GitHub Links]
    GithubLink --> Future2[Future: AI Extracts Skills from Projects]
    
    Future2 --> Dashboard[Access Dashboard]
    Dashboard --> SearchJobs[Search & Browse Jobs]
    Dashboard --> ViewSent[View Jobs Sent by Admin]
    
    ViewSent --> ApplyDecision{Apply or Reject?}
    ApplyDecision -->|Apply| SubmitApp[Submit Application]
    ApplyDecision -->|Reject| Dashboard
    
    SubmitApp --> Assessment{Assessment Required?}
    Assessment -->|Yes| TakeAssessment[Complete Assessment]
    Assessment -->|No| WaitShortlist[Wait for Shortlisting]
    TakeAssessment --> WaitShortlist
    
    WaitShortlist --> Shortlisted{Shortlisted?}
    Shortlisted -->|Yes| InterviewInvite[Receive Interview Invitation]
    Shortlisted -->|No| Dashboard
    
    InterviewInvite --> End([Interview Process])
    
    style Start fill:#e1f5e1
    style End fill:#ffe1e1
    style Future1 fill:#fff4e1
    style Future2 fill:#fff4e1
```

## 2. Employer User Flow

```mermaid
flowchart TD
    Start([Employer Visits Portal]) --> Signup[Sign Up]
    Signup --> EmailVerif{Email Verification}
    EmailVerif -->|Verified| CompanyInfo[Fill Company Information]
    EmailVerif -->|Not Verified| ResendEmail[Resend Verification Email]
    ResendEmail --> EmailVerif
    
    CompanyInfo --> CompanyType[Enter Company Type]
    CompanyType --> AboutCompany[Add Company Description]
    AboutCompany --> Dashboard[Access Employer Dashboard]
    
    Dashboard --> PostJob[Create New Job Post]
    PostJob --> JobTitle[Enter Job Title & Details]
    JobTitle --> JobDesc[Submit Job Description]
    JobDesc --> AISkills[System Suggests Skills Based on JD]
    
    AISkills --> ReviewSkills{Review & Modify Skills?}
    ReviewSkills -->|Modify| EditSkills[Add/Remove Skills]
    ReviewSkills -->|Accept| ExperienceReq[Set Experience Required]
    EditSkills --> ExperienceReq
    
    ExperienceReq --> SkillsRequired[Define Required Skills]
    SkillsRequired --> MarkMandatory[Mark Mandatory Skills]
    MarkMandatory --> PublishJob[Publish Job Post]
    
    PublishJob --> NotifyAdmin[Admin Receives Alert]
    NotifyAdmin --> WaitCandidates[Wait for Candidate Shortlist]
    
    WaitCandidates --> ReceiveShortlist[Receive Shortlisted Candidates from Admin]
    ReceiveShortlist --> ReviewCandidates[Review Candidate Profiles]
    ReviewCandidates --> ScheduleInterview[Schedule Interviews]
    
    ScheduleInterview --> ManageJobs[Manage Active Job Posts]
    ManageJobs --> Dashboard
    
    style Start fill:#e1f5e1
    style NotifyAdmin fill:#e1e8ff
    style PublishJob fill:#d4edda
```

## 3. Admin/HR Consultant User Flow

```mermaid
flowchart TD
    Start([Admin Login]) --> Dashboard[Admin Dashboard]
    Dashboard --> JobAlert{New Job Post Alert?}
    
    JobAlert -->|Yes| ViewJob[View Job Post Details]
    JobAlert -->|No| AccessPool[Access Candidate Pool]
    
    ViewJob --> AnalyzeReq[Analyze Job Requirements]
    AnalyzeReq --> FilterCandidates[Filter by Skills & Experience]
    
    AccessPool --> FilterCandidates
    
    FilterCandidates --> SetCriteria[Define Filter Criteria:<br/>- Required Skills<br/>- Experience Level<br/>- Skill Rating<br/>- Location]
    SetCriteria --> ApplyFilters[Apply Filters]
    
    ApplyFilters --> SortCandidates[Sort Candidates]
    SortCandidates --> SortBy{Sort By:}
    SortBy --> ExpSort[Experience]
    SortBy --> SkillSort[Skill Rating]
    SortBy --> MatchSort[Match Percentage]
    
    ExpSort --> ReviewList[Review Filtered List]
    SkillSort --> ReviewList
    MatchSort --> ReviewList
    
    ReviewList --> SelectCandidates[Select Candidates for Shortlist]
    SelectCandidates --> SendJobLink[Send Job Post Link to Candidates]
    
    SendJobLink --> TrackResponses[Track Candidate Responses]
    TrackResponses --> CandResponse{Candidate Response}
    
    CandResponse -->|Applied| SendAssessment{Assessment Required?}
    CandResponse -->|Rejected| UpdateStatus1[Update Status]
    
    SendAssessment -->|Yes| CreateAssessment[Create/Send Assessment]
    SendAssessment -->|No| ReviewApplications[Review Applications]
    
    CreateAssessment --> MonitorAssessment[Monitor Assessment Completion]
    MonitorAssessment --> EvaluateResults[Evaluate Assessment Results]
    EvaluateResults --> ReviewApplications
    
    ReviewApplications --> FinalShortlist[Create Final Shortlist]
    FinalShortlist --> SendToEmployer[Send Shortlist to Employer]
    
    SendToEmployer --> CoordinateInterviews[Coordinate Interview Process]
    CoordinateInterviews --> UpdateStatus2[Update Candidate & Job Status]
    UpdateStatus2 --> Dashboard
    UpdateStatus1 --> Dashboard
    
    style Start fill:#e1f5e1
    style SendToEmployer fill:#d4edda
    style CreateAssessment fill:#fff4e1
```

---

## More Diagrams explaining interactions

### 1. **Sequence Diagram** - Role Interactions
Shows how the three roles interact throughout the hiring process:

```mermaid
sequenceDiagram
    participant E as Employer
    participant A as Admin/HR
    participant C as Candidate
    participant S as System
    
    E->>S: Post Job with Requirements
    S->>A: Send Job Post Alert
    A->>S: Filter Candidate Pool
    S->>A: Return Matching Candidates
    A->>A: Review & Sort Candidates
    A->>C: Send Job Invitation
    C->>S: Apply to Job
    S->>A: Notify of Application
    A->>C: Send Assessment
    C->>S: Complete Assessment
    S->>A: Assessment Results
    A->>A: Create Final Shortlist
    A->>E: Send Shortlisted Candidates
    E->>C: Schedule Interview
```

### 2. **State Diagram** - Job Application States

```mermaid
stateDiagram-v2
    [*] --> Posted: Employer Posts Job
    Posted --> UnderReview: Admin Reviews
    UnderReview --> CandidatesInvited: Admin Sends to Candidates
    CandidatesInvited --> ApplicationReceived: Candidate Applies
    CandidatesInvited --> Rejected: Candidate Rejects
    ApplicationReceived --> AssessmentSent: Admin Sends Assessment
    AssessmentSent --> AssessmentCompleted: Candidate Completes
    AssessmentCompleted --> Shortlisted: Passed Assessment
    AssessmentCompleted --> NotShortlisted: Failed Assessment
    Shortlisted --> InterviewScheduled: Employer Reviews
    InterviewScheduled --> Hired: Interview Success
    InterviewScheduled --> NotSelected: Interview Failed
    Hired --> [*]
    NotSelected --> [*]
    Rejected --> [*]
    NotShortlisted --> [*]
```

### 3. **Complete Hiring Process**

```mermaid
flowchart TD
    subgraph Employer["👔 EMPLOYER"]
        E1[Post Job] --> E2[Define Requirements]
        E2 --> E3[Review Shortlist]
        E3 --> E4[Schedule Interviews]
    end
    
    subgraph Admin["👨‍💼 ADMIN/HR CONSULTANT"]
        A1[Receive Job Alert] --> A2[Filter Candidates]
        A2 --> A3[Send Invitations]
        A3 --> A4[Manage Assessments]
        A4 --> A5[Final Shortlist]
    end
    
    subgraph Candidate["👤 CANDIDATE"]
        C1[Receive Invitation] --> C2[Apply to Job]
        C2 --> C3[Complete Assessment]
        C3 --> C4[Wait for Interview]
    end
    
    E2 -.->|Job Details| A1
    A3 -.->|Job Link| C1
    C2 -.->|Application| A3
    C3 -.->|Results| A4
    A5 -.->|Candidates| E3
    E4 -.->|Interview Invite| C4
    
    style Employer fill:#e3f2fd
    style Admin fill:#f3e5f5
    style Candidate fill:#e8f5e9
```
