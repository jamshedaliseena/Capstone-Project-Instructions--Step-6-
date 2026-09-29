# StudyPath Capstone Project Proposal

## A Personalized Study Planner and Assignment Tracker

| Project detail | Information |
|---|---|
| Student | [Your name] |
| Course / Mentor | [Course name / mentor] |
| Submission date | [Date] |

## Project Summary

StudyPath is a responsive web application that helps students organize coursework, deadlines, and study sessions in one private workspace. A signed-in student can create courses and tasks, set due dates and priorities, record study sessions, and review upcoming work on a dashboard. The project will be built with Next.js and React, MongoDB, and authenticated API routes. The first release focuses on an individual planner; collaboration, AI-generated schedules, and institutional integrations are outside the initial scope.

## Problem and Rationale

Students often track assignments in several places, making it harder to see what is due soon and how much work remains. StudyPath addresses this organizational problem with a single interface that connects each task to a course, shows upcoming deadlines, and lets the student update progress. The project is appropriately scoped for a full-stack capstone because it includes CRUD operations, data ownership, authentication, API design, interface state, testing, deployment, and documentation.

## Goal and Measurable Objectives

The goal is to deliver a deployed, documented study planning application that protects each user’s data and supports the core planning workflow.

- Allow a user to register, sign in, sign out, and access only their own records.
- Provide create, view, update, and delete operations for courses and study tasks.
- Let a user set a task title, course, due date, priority, status, and optional notes.
- Show a dashboard with upcoming tasks and simple progress summaries.
- Validate user input and return clear success and error states in the interface.
- Document setup, environment variables, data model, API routes, testing, and deployment.

## Users and Use Cases

The primary user is a student planning their own coursework. A user should be able to create an account, add a course, record an assignment with a deadline, find tasks due soon, change a task’s status as work progresses, revise details, and remove completed or unwanted records. The initial release is designed for individual accounts.

## Scope and Requirements

### Included in the First Release

- Responsive dashboard and navigation for desktop and mobile screens.
- Authentication and authorization for private, user-owned data.
- Course and task management with full CRUD behavior.
- Dashboard views for upcoming, overdue, and completed tasks.
- Form validation, loading indicators, empty states, and actionable errors.
- Unit and integration tests for key data, API, and interface flows.
- Deployment to Render or a comparable hosting platform, with a documented setup guide.

### Outside the First Release

- Shared groups, instructor accounts, and course roster imports.
- Email, SMS, or push notifications.
- AI-generated study schedules or recommendations.
- Payments, public profiles, or third-party learning management integrations.

## Proposed Technical Design

The application will use Next.js with React components and server-side API routes. The API layer will validate requests, verify the signed-in user, and perform MongoDB operations. MongoDB access will be centralized in a reusable connection utility. The browser interface will use React state and context for session and screen-level state; persistent records will remain in MongoDB. Secrets and database credentials will be provided through environment variables and excluded from source control.

| Layer | Proposed choice | Responsibility |
|---|---|---|
| Web application | Next.js and React | Pages, shared layout, forms, dashboard, and navigation |
| API | Next.js REST API routes | Authentication checks, validation, CRUD, and consistent responses |
| Database | MongoDB | Users, courses, tasks, timestamps, and ownership |
| State | React Context and component state | Session, filters, form state, loading, and error feedback |
| Styling | CSS modules or global CSS | Responsive layout and accessible visual states |
| Hosting | Render or comparable platform | Production application and environment variables |

## Preliminary Data Model

| Entity | Key fields | Relationships and constraints |
|---|---|---|
| User | `_id`, `name`, `email`, `passwordHash`, `createdAt` | Email is unique; password is stored only as a secure hash. |
| Course | `_id`, `userId`, `name`, `code`, `color`, `createdAt` | Owned by one user; `userId` is required for every query. |
| Task | `_id`, `userId`, `courseId`, `title`, `description`, `dueDate`, `priority`, `status`, `createdAt`, `updatedAt` | Owned by one user; an optional course reference must also belong to that user. |

The initial model keeps the data small and directly supports the key workflows. The API will enforce ownership on every read and write, validate status and priority values, and prevent a task from referencing another user’s course. Dates will be stored consistently and displayed in the user’s local time zone.

## Preliminary API Plan

| Method and route | Purpose |
|---|---|
| `POST /api/auth/register` | Create a user account after validating input. |
| `POST /api/auth/login` | Authenticate a user and establish a session. |
| `POST /api/auth/logout` | End the current session. |
| `GET /api/courses` | List the signed-in user’s courses. |
| `POST /api/courses` | Create a course for the signed-in user. |
| `GET /api/courses/:id` | Read one owned course. |
| `PATCH /api/courses/:id` | Update an owned course. |
| `DELETE /api/courses/:id` | Delete an owned course, with an explicit policy for associated tasks. |
| `GET /api/tasks` | List owned tasks, with optional status, course, and date filters. |
| `POST /api/tasks` | Create a task after validating course ownership. |
| `GET /api/tasks/:id` | Read one owned task. |
| `PATCH /api/tasks/:id` | Update an owned task. |
| `DELETE /api/tasks/:id` | Delete an owned task. |

Protected routes will reject unauthenticated requests. Responses will use appropriate HTTP status codes and a predictable JSON shape. Detailed implementation choices, including the session library and deletion behavior for a course with tasks, will be finalized during development and recorded in the README.

## Security and Accessibility

- Hash passwords with a modern password-hashing library; never store plaintext passwords.
- Use secure, HTTP-only session cookies and production-appropriate cookie settings.
- Authorize every database operation by the authenticated user ID, including single-record routes.
- Validate and normalize user input on the server; do not rely only on browser validation.
- Keep secrets out of source control and document required environment variables without publishing their values.
- Use semantic headings and labels, keyboard-accessible controls, visible focus states, and sufficient color contrast.

## Implementation and Testing Plan

| Phase | Work products |
|---|---|
| Foundation | Next.js structure, shared layout, styling baseline, MongoDB connection, and configuration guide. |
| Data and API | User, course, and task models; validation; protected CRUD endpoints. |
| Authentication | Registration, login, logout, session handling, and authorization checks. |
| Interface | Dashboard, course/task forms, filters, progress states, responsive layout, and API integration. |
| Verification | Unit tests for validation and utilities; integration tests for API and ownership rules; manual end-to-end checks; defect fixes. |
| Release | Production deployment, environment setup, README, and submission review. |

Testing will cover successful and invalid inputs, missing sessions, attempts to access another user’s records, CRUD behavior, and the main student workflow from account creation through task completion. Before submission, the deployed application and setup instructions will be reviewed against the project requirements.

## Risks and Responses

| Risk | Response |
|---|---|
| Scope expands beyond available time | Keep collaboration, notifications, and AI features outside the first release. |
| Unauthorized access to records | Test ownership checks on list, detail, update, and delete operations. |
| Database or hosting configuration errors | Document environment variables and verify deployment using a clean setup. |
| Deadline or time-zone confusion | Store dates consistently, display local times clearly, and test boundary cases. |
| Insufficient test coverage | Prioritize authentication, authorization, validation, and core CRUD workflows. |

## Completion Criteria

- A deployed user can register, sign in, and manage courses and tasks.
- Users cannot read or change other users’ private records.
- Dashboard and CRUD flows work on mobile and desktop with clear validation and error feedback.
- Core unit and integration tests pass, and critical workflow defects are resolved.
- README explains prerequisites, setup, environment configuration, architecture, API, testing, and deployment.
- The submission includes the initial project ideas and proposal materials requested by the capstone instructions.

## Expected Deliverables

- Source code organized as a Next.js application.
- MongoDB-backed REST API and authentication/authorization implementation.
- Responsive React interface with CRUD workflows.
- Unit and integration test coverage for core behavior.
- Deployed application URL and setup/deployment documentation.
- README and finalized capstone submission materials.
