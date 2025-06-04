# GitHub Action Usage and Automatic Updates

This repository demonstrates how to use custom GitHub Actions from a shared repository and keep them automatically updated using GitHub Dependabot.

## How Custom Actions are Fetched

In the [demo workflow](.github/workflows/demo-workflow.yaml), we're referencing a custom action from a shared repository:

```yaml
- name: Run Example Action
  uses: Gholie-Private-Actions-Demo/public-shared/.github/actions/example-action@example-action-v1.0.0
  with:
    message: 'This is a custom message'
    importance: 'high'
```

### Breakdown of the Action Reference

- `Gholie-Private-Actions-Demo/public-shared`: The GitHub organization and repository where the shared action is stored
- `.github/actions/example-action`: The path within that repository to the specific action
- `@example-action-v1.0.0`: The tag or version of the action to use

When the workflow runs, GitHub automatically fetches the action from the specified repository at the given version tag. This allows for versioned, reusable actions across multiple repositories.

## Automatic Updates with Dependabot

The [dependabot.yaml](.github/dependabot.yaml) file in this repository configures GitHub Dependabot to automatically check for updates to the GitHub Actions used in the workflows:

```yaml
version: 2
updates:
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "daily"
```

### How Dependabot Works with GitHub Actions

1. **Daily Checks**: Dependabot checks daily for new versions of all actions used in your workflows
2. **Pull Request Creation**: When a new version of an action is detected, Dependabot creates a pull request to update the reference
3. **Versioned Updates**: Updates maintain the versioning pattern you've established (e.g., `@v1.0.0` to `@v1.0.1` or `@v1.1.0`)

### Benefits

- **Security**: Automatically get security fixes and patches
- **Features**: Stay updated with the latest features of shared actions
- **Reduced Maintenance**: No need to manually track and update action versions

## Using Private Actions (Currently Commented Out)

The dependabot.yaml file includes commented configuration for using private actions:

```yaml
# registries:
#   private-actions:
#     type: git
#     url: https://github.com/
#     username: x-access-token
#     password: ${{ secrets.<your-dependabot-secret> }}
```

To use private actions, you would:

1. Uncomment these sections
2. Configure a GitHub secret with appropriate access tokens
3. Reference the registry in the updates section

## Additional Configuration Options

Dependabot can be further configured with:

- **Allow Lists**: Specify which dependencies to update
- **Ignore Lists**: Exclude certain dependencies from updates
- **Version Constraints**: Control which version ranges to consider
- **Labels and Assignees**: Customize the created pull requests

For more information, see the [GitHub Dependabot documentation](https://docs.github.com/en/code-security/dependabot/dependabot-version-updates/configuration-options-for-the-dependabot.yml-file).
