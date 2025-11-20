# Repository Automation

This document describes the automated workflows configured for the Starknet PM repository.

## README Gallery Auto-Update

### Overview
The repository includes a GitHub Actions workflow that automatically updates the "Recent Meetings" gallery in README.md whenever new meeting screenshots are added.

### How It Works

1. **Trigger**: The workflow runs automatically when:
   - A new call screenshot is added to `AllCoreDevs-Full-Nodes-Meetings/images/`
   - A meeting notes file is updated in `AllCoreDevs-Full-Nodes-Meetings/`
   - Manually triggered via GitHub Actions UI

2. **Process**:
   - Identifies the 3 most recent call screenshots
   - Extracts meeting dates from the corresponding meeting notes files
   - Updates the README.md "Recent Meetings" section
   - Commits and pushes changes automatically

3. **Benefits**:
   - No manual README updates needed
   - Gallery always shows latest 3 meetings
   - Dates automatically extracted from meeting notes
   - Consistent formatting
   - Zero maintenance overhead

### Workflow File
Location: `.github/workflows/update-readme-gallery.yml`

### Manual Trigger
You can manually trigger the workflow from the GitHub Actions tab:
1. Go to Actions → Update README Gallery
2. Click "Run workflow"
3. Select branch and run

### Troubleshooting

**Workflow not running?**
- Check that the workflow file has proper permissions (`contents: write`)
- Ensure you're pushing to the main branch
- Verify the file paths match the trigger patterns

**Gallery not updating?**
- Ensure screenshot files follow naming convention: `call_XXX_attendees.png`
- Verify meeting notes exist with proper date format
- Check workflow logs in the Actions tab

### Technical Details

**Python Dependencies**: None (uses stdlib only)
**Permissions Required**: `contents: write`
**Runs On**: `ubuntu-latest`
**Python Version**: `3.x`

### Date Extraction
The workflow automatically extracts dates from meeting notes by looking for:
```
**Date & Time:** Thursday, November 6, 2025
```

And converts them to the format: `Nov 6, 2025`

### Skipping CI
The automated commit includes `[skip ci]` to prevent triggering other CI workflows unnecessarily.

---

**Maintainer**: Aayush Giri ([@Giri-Aayush](https://github.com/Giri-Aayush))
