# Setup Instructions for Repository Admins

## ⚠️ Required: GitHub Token Configuration

For the automatic issue sync to work, you need to create a GitHub Personal Access Token (PAT) and add it as a secret.

### Steps:

1. **Create a Personal Access Token**
   - Go to GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
   - Or visit: https://github.com/settings/tokens
   - Click "Generate new token (classic)"
   - Name it: `ProcureAI Feedback Sync`
   - Set expiration: 1 year (or "No expiration" if you prefer)
   - Select these scopes:
     - ✅ `repo` (Full control of private repositories)
       - This includes: `repo:status`, `repo_deployment`, `public_repo`, `repo:invite`
     - ✅ `workflow` (Update GitHub Action workflows)
   - Click "Generate token"
   - **Copy the token immediately** (you won't see it again!)

2. **Add Token to Feedback Repository**
   - Go to: https://github.com/Procure-AI/ProcureAI-feedback/settings/secrets/actions
   - Click "New repository secret"
   - Name: `BACKEND_SYNC_TOKEN`
   - Value: Paste the token you copied
   - Click "Add secret"

3. **Verify Setup**
   - Create a test issue in the feedback repo
   - Check that it automatically appears in ProcureAI-backend with the "external" and "backlog" labels
   - The bot should comment on the feedback issue with a link to the backend issue

## How It Works

When someone submits an issue in `ProcureAI-feedback`:
1. GitHub Actions automatically triggers
2. A corresponding issue is created in `ProcureAI-backend` with:
   - Same title and description
   - Labels: `external`, `backlog`, plus any labels from the template
   - Link back to the original feedback issue
3. A comment is added to the feedback issue with a link to the internal issue
4. Future edits, labels, and status changes sync automatically

## Troubleshooting

### Issue not syncing?
- Check that `BACKEND_SYNC_TOKEN` secret is set correctly
- Verify the token has `repo` and `workflow` scopes
- Check the Actions tab in the feedback repo for error messages

### Want to disable auto-sync temporarily?
- Go to feedback repo Settings → Actions → Disable Actions

### Need to manually sync an old issue?
- Close and reopen the feedback issue to trigger the workflow

## Customization

### Change target repository
Edit `.github/workflows/sync-to-backend.yml` and update:
```javascript
const backendOwner = 'Procure-AI';
const backendRepo = 'ProcureAI-backend';
```

### Change default labels
Edit the workflow and modify the labels array:
```javascript
labels: ['external', 'backlog', ...feedbackIssue.labels.map(l => l.name)],
```

### Add to specific project board
You can enhance the workflow to automatically add issues to a GitHub Project board by adding project API calls.

---

**Setup completed?** You're ready to accept external feedback! 🎉

Share this URL with external users: https://github.com/Procure-AI/ProcureAI-feedback/issues/new/choose
