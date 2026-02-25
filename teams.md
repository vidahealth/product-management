# Team Configuration

Maps team names to their Jira and Confluence defaults. Agents read this file to apply the correct project key, labels, and Confluence location automatically.

To add a team: add a row to each table and open a PR.

---

## Jira

| Team | Space (Project Key) | Default Labels |
|------|---------------------|----------------|
| Data Platform | Platform | data-platform |

---

## Confluence

| Team | Space Key | Parent Page |
|------|-----------|-------------|
| Data Platform | DATA | Data Platform Team |

---

## Notes

- **Jira Space (Project Key)** is the prefix used in issue keys (e.g., `PLAT-123`). Find it in Jira project settings.
- **Default Labels** are applied to every Epic, Story, and Sub-Task created for this team. Comma-separate multiple labels.
- **Confluence Space Key** is the short code for the team's Confluence space. Find it in the Confluence space settings URL.
- **Confluence Parent Page** is the exact title of the page under which initiative docs are nested. The agent searches for it by title.
- If a team is not listed, the agent will ask for the values and offer to add the row.
