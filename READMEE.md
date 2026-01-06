# Docker Backdated Tag Generator

A simple script to push Docker images to a **virtual** docker repo and generate tags that can be backdated in the database for testing cleanup policies.

This is especially useful for reproducing **time-based artifact cleanup** scenarios, where images need to appear older than the policy threshold.

---

## What it does

- Pulls a source Docker image (default: `hello-world:latest`) from a registry.  
- Tags the image with a **base version** and **unique tags**.  
- Pushes multiple versions to a **virtual repo**.  
- Optionally removes the pushed images locally to save disk space.  
- Prints an **SQL query** to backdate the `created`, `modified`, and `updated` timestamps in the Artifactory database.

---

## What it expects

- **Virtual repository name** — the repository to push to (required).  
- **Local deployment repository** — defaults to `<virtual_repo>-local`, used in the DB query.  
- **Docker registry** — defaults to `localhost:8082`.  
- **Docker credentials** — username and password for login.  
- **Source image** — defaults to `hello-world:latest`.  
- **Base tag** — defaults to `2.1.0`.  
- **Number of versions to generate** — defaults to 15.  
- **Optional cleanup** — whether to remove pushed images locally.

---

## SQL Query

At the end of the run, the script prints a query to set the pushed images’ timestamps older than a year:

```sql
UPDATE nodes
SET
    created = '1721850900000',
    modified = '1721850900000',
    updated = '1721850900000'
WHERE
    repo = '<local_repo>' AND
    node_path LIKE 'custom-docker-image%';

