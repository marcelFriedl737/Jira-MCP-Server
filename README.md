# Jira MCP Server

An MCP server that provides tools for managing Jira projects, issues, and workflows. The server enables:

- Project creation and configuration
- Issue and subtask management
- Issue linking and dependencies
- Automated issue workflows

## Configuration

Required environment variables:

- `JIRA_HOST`: Your Jira instance hostname
- `JIRA_EMAIL`: Your Jira account email
- `JIRA_API_TOKEN`: API token from https://id.atlassian.com/manage-profile/security/api-tokens

## Available Tools

### 1. User Management

```typescript
// Get user's account ID by email
{
  email: "user@example.com";
}
```

### 2. Issue Type Management

```typescript
// List all available issue types
// Returns: id, name, description, subtask status
// No parameters required
```

### 3. Issue Link Types

```typescript
// List all available issue link types
// Returns: id, name, inward/outward descriptions
// No parameters required
```

### 4. Issue Management

#### Retrieving Issues

```typescript
// Get all issues in a project
{
  projectKey: "PROJECT"
}

// Get issues with JQL filtering
{
  projectKey: "PROJECT",
  jql: "status = 'In Progress' AND assignee = currentUser()"
}

// Get issues assigned to user
{
  projectKey: "PROJECT",
  jql: "assignee = 'user@example.com' ORDER BY created DESC"
}
```

#### Creating Issues

```typescript
// Create a standard issue
{
  projectKey: "PROJECT",
  summary: "Issue title",
  issueType: "Task",  // or "Story", "Bug", etc.
  description: "Detailed description",
  assignee: "accountId",  // from get_user tool
  labels: ["frontend", "urgent"],
  components: ["ui", "api"],
  priority: "High"
}

// Create a subtask
{
  parent: "PROJECT-123",
  projectKey: "PROJECT",
  summary: "Subtask title",
  issueType: "Subtask",
  description: "Subtask details",
  assignee: "accountId"
}
```

#### Updating Issues

```typescript
// Update issue fields
{
  issueKey: "PROJECT-123",
  summary: "Updated title",
  description: "New description",
  assignee: "accountId",
  status: "In Progress",
  priority: "High"
}
```

#### Issue Dependencies

```typescript
// Create issue link
{
  linkType: "Blocks",  // from list_link_types
  inwardIssueKey: "PROJECT-124",  // blocked issue
  outwardIssueKey: "PROJECT-123"  // blocking issue
}
```

#### Deleting Issues

```typescript
// Delete single issue
{
  issueKey: "PROJECT-123",
  deleteSubtasks: false
}

// Delete issue with subtasks
{
  issueKey: "PROJECT-123",
  deleteSubtasks: true
}
```

## Field Formatting

### Description Field

The description field supports markdown-style formatting:

- Use blank lines between paragraphs
- Use "- " for bullet points
- Use "1. " for numbered lists
- Use headers ending with ":" (followed by blank line)

Example:

```
Task Overview:

This task involves implementing new features:
- Feature A implementation
- Feature B testing

Steps:
1. Design component
2. Implement logic
3. Add tests

Acceptance Criteria:
- All tests passing
- Documentation updated
```

## Error Handling

The server provides detailed error messages for:

- Invalid issue keys
- Missing required fields
- Permission issues
- API rate limits

## References

- [Jira REST API Documentation](https://docs.atlassian.com/software/jira/docs/api/REST/7.12.0)
- [Jira REST API Examples](https://developer.atlassian.com/server/jira/platform/jira-rest-api-examples/)
