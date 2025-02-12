# Stash the changes
git stash push -m "Saving changes before switching branches"

# Create and switch to the new branch
git checkout -b feature/Marvel_25.2.1_NodeClose_Summary

# Apply the latest stash
git stash apply

# Drop the stash after applying it (optional)
git stash drop
