# Reproduction: Setting globally required `packageRules`, while allowing opt-in configuration

Via https://github.com/renovatebot/renovate/discussions/19166

# Expected

The repo's `renovate.json` - which includes the preset `default.json` which has `ignorePresets` set - should ignore the `not-onboarded` preset, and detect that there's a `Dockerfile`:

```
 INFO: Repository started (repository=jamietanna/renovate-config-19166-repo)
       "renovateVersion": "34.98.0"
 INFO: Dependency extraction complete (repository=jamietanna/renovate-config-19166-repo, baseBranch=main)
       "stats": {
         "managers": {"dockerfile": {"fileCount": 1, "depCount": 1}},
         "total": {"fileCount": 1, "depCount": 1}
       }
 ...
```

# Actual

No dependencies are found (as the enabled `regex` manager has no config).

```
 INFO: Repository started (repository=jamietanna/renovate-config-19166-repo)
       "renovateVersion": "34.98.0"
 INFO: Dependency extraction complete (repository=jamietanna/renovate-config-19166-repo, baseBranch=main)
       "stats": {"managers": {}, "total": {"fileCount": 0, "depCount": 0}}
 INFO: Issue created (repository=jamietanna/renovate-config-19166-repo)
 INFO: Repository finished (repository=jamietanna/renovate-config-19166-repo)
       "cloned": true,
       "durationMs": 5364
```

# Other notes

The expected behaviour can be

```diff
diff --git config.js config.js
index 3ebcc90..96d36ff 100644
--- config.js
+++ config.js
@@ -4,4 +4,7 @@ module.exports = {
   extends: [
     "local>jamietanna/renovate-config-19166:not-onboarded",
   ],
+  ignorePresets: [
+    "local>jamietanna/renovate-config-19166:not-onboarded",
+  ]
 }
```
