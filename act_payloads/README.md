 

# Workflow test payloads for act

### [test_pr.json](test_pr.json)

For `validate_title` job

```sh
act -j validate-title -e act_payloads/test_pr.json -s GITHUB_TOKEN="$(gh auth token)"
```

