# Switching Environments

When you are ready to deploy to production all it takes is an environment switch.

```bash
firebase use production; # switches all future firebase commands to production
firebase deploy
```

Or use the -P flag

```bash
firebase -P ${REACT_APP}-production deploy; # Does not switch default environment but still deploys to production
```

