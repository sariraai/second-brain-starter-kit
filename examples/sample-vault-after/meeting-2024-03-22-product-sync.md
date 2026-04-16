---
title: "Product Sync — March 22"
type: meeting
date: 2024-03-22
tags: [work, meetings, onboarding, auth-service, api-migration]
attendees: [Sarah, Jamie, Marcus, Lisa]
location: notes/meetings/
---

# Product Sync — March 22

lisa presented the new onboarding mockups. looks good but:
- too many steps (7 screens before you see the dashboard)
- the progress bar helps but people still drop off at step 4 (payment info)
- suggested: let users skip payment and do a 14-day trial instead

marcus update on auth service:
- new oauth flow ready for testing next week
- will unblock our API migration
- needs us to update our token refresh logic

my update:
- monitoring dashboards 80% done
- found a memory leak in the chart rendering (assigned to jamie)

action items:
- [ ] me: review lisa's mockups and send feedback by monday
- [ ] jamie: investigate memory leak, report back thursday
- [ ] marcus: share oauth testing environment credentials
- [ ] sarah: schedule design review for onboarding flow

next meeting: march 29

## Related

- [[meeting-2024-03-15-dev-team]] — API migration blocked on auth; marcus now says unblocked next week
- [[q2-planning-notes]] — onboarding completion rate target 60% to 75%; trial flow could help
