# Lab 1 submission

## Task 1 — SSH Commit Signing & First Signed Commit

### QuickNotes curl outputs

**GET /health:**
```json
{"notes":4,"status":"ok"}
```

**GET /notes (before POST):**
```json
[{"id":4,"title":"Endpoint cheat-sheet","body":"GET /notes  GET /notes/{id}  POST /notes  DELETE /notes/{id}  GET /health  GET /metrics","created_at":"2026-01-15T10:15:00Z"},{"id":1,"title":"Welcome to QuickNotes","body":"This is the project you'll containerize, deploy, monitor, and harden across all 10 labs.","created_at":"2026-01-15T10:00:00Z"},{"id":2,"title":"Read app/main.go first","body":"Start by understanding the entry point — env vars, signal handling, graceful shutdown.","created_at":"2026-01-15T10:05:00Z"},{"id":3,"title":"DevOps mantra","body":"If it hurts, do it more often.","created_at":"2026-01-15T10:10:00Z"}]
```

**POST /notes:**
```json
{"id":5,"title":"hello","body":"first POST","created_at":"2026-09-11T10:27:03.3976101Z"}
```

**GET /notes (after POST):**
```json
[{"id":1,"title":"Welcome to QuickNotes","body":"This is the project you'll containerize, deploy, monitor, and harden across all 10 labs.","created_at":"2026-01-15T10:00:00Z"},{"id":2,"title":"Read app/main.go first","body":"Start by understanding the entry point — env vars, signal handling, graceful shutdown.","created_at":"2026-01-15T10:05:00Z"},{"id":3,"title":"DevOps mantra","body":"If it hurts, do it more often.","created_at":"2026-01-15T10:10:00Z"},{"id":4,"title":"Endpoint cheat-sheet","body":"GET /notes  GET /notes/{id}  POST /notes  DELETE /notes/{id}  GET /health  GET /metrics","created_at":"2026-01-15T10:15:00Z"},{"id":5,"title":"hello","body":"first POST","created_at":"2026-09-11T10:27:03.3976101Z"}]
```

### Git signature verification

```bash
$ git log --show-signature -1
commit 41ac627fe7e31c7b36f736f527958ce2759a4305
Good "git" signature with ED25519 key SHA256:phZXUR5LZjKAehRCJ6LA6xc4dtzfoyoHpquQFhq0ibg
Author: uSs3ewa <m.panchenko@innopolis.university>
Date:   Fri Sep 11 13:35:52 2026 +0300

    docs(lab1): start submission
    
    Signed-off-by: uSs3ewa <m.panchenko@innopolis.university>
```

### Why signed commits matter

Signed commits are crucial for supply chain security in DevOps. The xz-utils backdoor incident in March 2024 demonstrated how malicious actors can compromise critical infrastructure through social engineering and code injection. By requiring signed commits, we establish cryptographic proof of author identity, making it significantly harder for attackers to inject malicious code undetected. This practice is especially important in collaborative projects where multiple contributors have write access, as it provides traceability and accountability for every change.

### Verified badge screenshot

*Screenshot of the Verified badge on GitHub commit page will be added after PR is opened*

## Task 2 — Pull Request Template & First PR

The PR template has been added to `.github/pull_request_template.md` on the main branch. The template includes:
- Goal section for PR description
- Changes section for listing modifications
- Testing section for verification steps
- Checklist for ensuring quality standards

## Task 3 — GitHub Community Engagement

### GitHub Community

Starring repositories is important in open source because it helps bookmark interesting projects for future reference, indicates project popularity and community trust, and encourages maintainers by showing user interest. Following developers helps in team projects and professional growth by allowing you to see what others are working on, discover new projects through their activity, build professional connections beyond the classroom, and stay updated on classmates' work for future collaboration opportunities.

### Actions completed:
- ⭐ Starred the course repository (inno-devops-labs/DevOps-Intro)
- ⭐ Starred simple-container-com/api project
- 👨‍💻 Following professor: @Cre-eD
- 👨‍💻 Following TA: @Naghme98
- 👨‍💻 Following TA: @pierrepicaud
- 👨‍💻 Following 3+ classmates