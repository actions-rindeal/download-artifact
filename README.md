# ⬇️📦🚀 Download Artifact Action

This action downloads build artifacts in your GitHub Actions workflow.
It's a wrapper around [actions/download-artifact](https://github.com/actions/download-artifact)
with an additional `artifact` input, which accepts a JSON string with extended artifact metadata.

To be paired with the [upload-artifact](https://github.com/actions-rindeal/upload-artifact) action,
which is again a wrapper around [actions/upload-artifact](https://github.com/actions/upload-artifact)
with an additional `artifact` output that provides the extended artifact metadata JSON string.

## 🌟 How is this useful?

Both actions together allow you to easily perform cross-workflow-run or even cross-repository artifact transfers
with just a single variable being passed around.

## 📋 Usage

```yaml
- name: "Download artifact"
  id: 'DOWNLOAD'
  uses: 'actions-rindeal/download-artifact@v4'
  with:
    'artifact': ${{ steps['UPLOAD'].outputs['ARTIFACT'] }}
    'path': 'my-artifact'
    # will download the artifact to `${GITHUB_WORKSPACE}/my-artifact`
```

Example of accessing the metadata props:
```yaml
env:
  'ARTIFACT_NAME': ${{ fromJSON(steps['DOWNLOAD'].outputs['ARTIFACT']).name }}
```

## 🔧 Inputs

| Name             | Description                                                                 | Default          |
|------------------|-----------------------------------------------------------------------------|------------------|
| `artifact`       | Artifact metadata JSON as output by the actions-rindeal/upload-artifact action | `''`             |
| `name`           | Name of the artifact to download. If unspecified, all artifacts for the run are downloaded. | `''`             |
| `path`           | Destination path. Supports basic tilde expansion. Defaults to $GITHUB_WORKSPACE | `''`             |
| `pattern`        | A glob pattern matching the artifacts that should be downloaded. Ignored if name is specified. | `''`             |
| `merge-multiple` | When multiple artifacts are matched, this changes the behavior of the destination directories. If true, the downloaded artifacts will be in the same directory specified by path. If false, the downloaded artifacts will be extracted into individual named directories within the specified path. | `'false'`        |
| `github-token`   | The GitHub token used to authenticate with the GitHub API. This is required when downloading artifacts from a different repository or from a different workflow run. | `''`             |
| `repository`     | The repository owner and the repository name joined together by "/". If github-token is specified, this is the repository that artifacts will be downloaded from. | `''`             |
| `run-id`         | The id of the workflow run where the desired download artifact was uploaded from. If github-token is specified, this is the run that artifacts will be downloaded from. | `''`             |

## 📤 Outputs

| Name           | Description                | Example                                                      |
|----------------|----------------------------|--------------------------------------------------------------|
| `download-path`| Path of artifact download  | `'/home/runner/work/my-repo/my-repo/my-artifact'`            |

## 📚 Notes

For more details on artifact handling in GitHub Actions, refer to the [official documentation](https://docs.github.com/en/actions/using-workflows/storing-workflow-data-as-artifacts).
