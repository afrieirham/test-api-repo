## How to make GitHub commit programatically

```
Source: https://blog.apihero.run/how-to-programmatically-create-a-commit-on-github#heading-1-fetch-latest-commit
```

**1. Fetch latest commit to get reference**

```
GET /repos/{{owner}}/{{repo}}/git/ref/heads/{{branch}}
```

> Save `body.object.sha` from response as `reference_sha`.


**2. Create a tree**

```
POST /repos/{{owner}}/{{repo}}/git/trees
```

Body
```json
{
  "base_tree": "{{reference_sha}}",
  "tree": [
    {
      "path": "{{filename}}",
      "mode": "100644",
      "type": "blob",
      "content": "hello world"
    }
  ]
}
```

> Save `body.sha` from response as `tree_sha`.

**3. Create a commit**

```
POST /repos/{{owner}}/{{repo}}/git/commits
```

Body
```json
{
  "message": "commit message",
  "parents": ["{{reference_sha}}"],
  "tree": "{{tree_sha}}"
}
```

> Save `body.sha` from response as `commit_sha`.

**4. Update the reference**

```
PATCH /repos/{{owner}}/{{repo}}/git/refs/heads/{{branch}}
```

Body
```json
{
  "sha": "{{commit_sha}}"
}
```

DONE! 🥳
