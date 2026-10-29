# IT Student Participation Certificate Portal — Prototype

## Included
- Responsive student/faculty portal UI
- Demo role switcher (Faculty / Student)
- Certificate submission form with PDF/JPG/PNG attachment
- Pending / Approved / Rejected workflow
- Search and filters by status, section, and event type
- Student-wise, event-wise, section-wise, month-wise, and full-record CSV exports
- Demo data and local browser persistence

## Run the prototype
1. Extract the ZIP.
2. Open `index.html` in a modern browser.
3. Use **Switch to Student View** to add a demo submission.
4. Switch to **Faculty View** to approve/reject submissions.
5. Use **Reports & Export** to download Excel-compatible CSV files.

## Important limitations
This is a front-end prototype, not a production-ready multi-user system. Data and certificate files are stored in the current browser's local storage and are not shared between students/faculty or devices. The role switch is only a demonstration and is NOT authentication. Do not use real student data/certificates in this prototype.

## Production implementation plan
Recommended stack: React/Next.js frontend, Supabase Auth + PostgreSQL + private Storage, deployed to a college-approved hosting environment.

1. **Student identity:** Import an approved IT student roster (roll number, name, year, section, college email). Authenticate with college email/OTP or a verified one-time onboarding process. Roll number and name alone are not secure credentials.
2. **Roles:** Server-side student and faculty roles; students can create/view only their own submissions. Designated faculty/admin can review records. Never rely on a client-side role selector.
3. **Database:** `students`, `submissions`, and `review_events` tables. Each submission should include event name, organizer, event date, category, participation level, achievement, file path, status, timestamps, and reviewer notes.
4. **Storage:** Private certificate bucket; allow PDF/JPG/PNG only, enforce size limits, and use short-lived signed URLs. Validate file type/size on the server.
5. **Workflow:** New submissions default to Pending. Faculty can approve/reject with a reason; keep an audit trail of reviewer, time, and status changes. Students can see the decision and rejection note.
6. **Reports:** Filtered reports and CSV/XLSX exports for student-wise, event-wise, section-wise, month-wise, and all submissions. Clarify whether department reports count all submissions or only approved ones.
7. **Safeguards:** Restrict access to the IT Department roster, protect personal data, back up the database, log administrative actions, and test authorization before launch.
8. **Launch checklist:** Confirm official roster, faculty reviewers, academic-year/section rules, retention policy, hosting approval, and who administers access.

## Suggested database fields
- `students`: id, roll_number (unique), full_name, college_email, academic_year, semester, section, active
- `submissions`: id, student_id, event_name, organizer, event_date, category, participation_level, achievement, certificate_storage_path, status, notes, created_at, updated_at
- `review_events`: id, submission_id, reviewer_id, previous_status, new_status, review_note, reviewed_at

Before deployment, configure server-side authorization policies so a student cannot access another student's record or certificate, and only approved faculty can review submissions.


## Certificate downloads (v2 update)
- In the Submissions table and dashboard, click **Download certificate** when a certificate file is attached.
- The details view also includes a download action and preview link.
- The prototype can download only files uploaded into that same browser session/storage; demo records without an attachment cannot be downloaded.
