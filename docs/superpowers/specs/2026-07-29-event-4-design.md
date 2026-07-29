# Event 4 Bilingual Report Design

## Goal

Add a fourth bilingual event report for **AWS Vietnam Community Meetup: AI Revolution, Open-Source Agents & Engineering Excellence**, matching the existing Event 1–3 structure without inventing evidence.

## Confirmed Event Information

- Date: Saturday, 25/07/2026.
- Time: 08:30–12:00.
- Check-in: 08:30.
- Program start: 09:00.
- Location: AWS Hà Nội, 7th Floor, Grand Terra Tower, 36 Cát Linh, Đống Đa, Hà Nội.
- Role: In-person attendee.
- Speakers: Henry (Đức) Bùi, Nguyễn Thu (Yuna), Tuấn Vũ, and Nam Lã.
- Evidence: one real participation photo will be supplied later.

## Pages and Navigation

Create matching English and Vietnamese pages under `content/4-EventParticipated/4.4-Event4/` with front-matter weight `4` and numbering `4.4`.

Update both Event section indexes to:

- State that four events were attended.
- Add Event 4 to the event list.
- Add its date, location, and in-person-attendee role to the summary table.

Update Event 3 navigation to link to Event 4. Event 4 links back to Event 3 and the Events Participated index.

## Event 4 Content

Both language versions use equivalent meaning and the established report style:

1. Event information table.
2. Overview and event objectives.
3. Speakers and roles.
4. Four key sessions:
   - Ship Fast with AI, Not by AI and the Outer Loop.
   - AI trends to business value.
   - OpenClaw and open-source agent runtimes.
   - AI-native infrastructure from an infrastructure-engineering perspective.
5. Key takeaways covering design mindset, technical architecture, and modernization.
6. Connection to the internship project, especially trajectory evidence, deterministic safety controls, human review, audit logs, scoped permissions, and verification.
7. Event experience.
8. Lessons learned.
9. Participation evidence.

## Evidence Boundary

Do not create a placeholder image file, broken image URL, fictional caption, or claim that a specific photograph is already embedded. Until the user supplies the real image path, the evidence section states only that one in-person participation photo will be added. Adding the image later requires one real asset plus matching EN/VI Markdown references and captions.

## Verification

- Run a fresh Hugo build and require exit code `0`.
- Confirm both new routes are generated.
- Confirm Event 3 links forward to Event 4 in both languages.
- Confirm Event 4 links backward and to the correct language-specific index.
- Confirm EN/VI indexes state four events and include the Event 4 summary row.
- Confirm no nonexistent Event 4 image is referenced.
