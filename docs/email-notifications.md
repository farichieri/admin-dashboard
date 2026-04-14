## Overview

Users should receive email notifications when important events happen in their workspaces, so they don't have to constantly check the dashboard. Notifications must be opt-in per category and respect timezone preferences.

## User stories

-   As a developer, I want to get notified by email when a task is assigned to me, so I can start working on it without having to poll the UI.
    
-   As a tech lead, I want to get notified when a new requirement is generated from a document in my project, so I can review it while the context is fresh.
    
-   As an admin, I want to receive a daily digest of workspace activity instead of individual notifications, so my inbox doesn't get flooded.
    
-   As any user, I want to opt out of specific notification categories from my account settings.
    

## Functional requirements

1.  Each user has notification preferences per category: `task_assigned`, `requirement_generated`, `task_updated`, `mentioned_in_comment`, `daily_digest`.
    
2.  Preferences default to `enabled` for `task_assigned` and `mentioned_in_comment`, `disabled` for the rest.
    
3.  Emails must render the user's name, the workspace/project name, the event detail, and a deep link back to the relevant page.
    
4.  Each email must include a one-click unsubscribe link for that specific category that works without the user being logged in.
    
5.  Daily digest runs at 9am in the user's timezone and includes all activity from the last 24 hours across workspaces they're a member of.
    
6.  All outbound emails must be sent through our existing Resend integration — we should not add a second email provider.
    

## Non-functional requirements

-   Notifications must not block user-facing requests — enqueue to the worker and return immediately.
    
-   If an email fails to send after 3 retries, log the error and move on — do not keep retrying indefinitely.
    
-   Digest generation must complete within 5 minutes for workspaces with up to 100 members.
    

## Out of scope

-   SMS, push, or Slack notifications (future)
    
-   In-app notification center UI (separate feature)
    
-   Admin controls to force-enable notifications for all workspace members