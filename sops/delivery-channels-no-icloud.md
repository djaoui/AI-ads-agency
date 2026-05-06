# SOP: Delivery Channels — Never iCloud

iCloud Drive sync to mobile is unreliable (sometimes hours-long delay) and should NOT be used for any production deliverable.

## Approved delivery channels

| Channel | Persistence | Use case |
|---|---|---|
| **Atlas Cloud upload** (`atlas_upload_media`) | 24h CDN URL | Default for video/image deliverables to user mobile |
| **Gofile.io** (`upload.gofile.io/uploadFile`) | ~7 days | Backup / longer-lived URL |
| **GitHub repo** (`AI-ads-agency`) | Permanent, version-controlled | Documents, prompts, brand bibles, SOPs |
| **Slack DM / channel** (`slack_send_message`) | Workspace-permanent | Preferred for step-by-step validation flow |
| **ShareOnboardingGuide** | Anthropic-hosted permanent | One-off shareable Claude Code onboarding |

## Forbidden
- iCloud Drive (`~/Library/Mobile Documents/com~apple~CloudDocs/`) — unreliable mobile sync. Never use.
- tmpfiles.org — 60-min expiry, often expired before user reads. Use only as last fallback.
