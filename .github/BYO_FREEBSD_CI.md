# BYO FreeBSD CI image

rdma-core FreeBSD jobs use [cross-platform-actions/action](https://github.com/cross-platform-actions/action) with a **self-built** qcow2 from [slavashw/freebsd-builder](https://github.com/slavashw/freebsd-builder), not AnyVM or CPA default images.

## One-time setup (GitHub)

1. Create `slavashw/freebsd-builder` (empty public repo) if it does not exist.
2. From a machine with GitHub SSH or `gh` auth, push the prepared builder tree:

   ```bash
   cd /path/to/slava_github_freebsd-builder   # or clone after you push it once
   git remote add slavashw git@github.com:slavashw/freebsd-builder.git
   git push -u slavashw main
   ./scripts/publish-byo-image.sh
   ```

3. In GitHub Actions for **freebsd-builder**, wait for the **Build VM Disk Image** workflow on tag `v15.1.0`. Publish the draft release.

4. Confirm the asset responds:

   ```bash
   curl -fIL --range 0-0 \
     https://github.com/slavashw/freebsd-builder/releases/download/v15.1.0/freebsd-15.1-x86-64.qcow2
   ```

5. Push the updated [`.github/workflows/freebsd-ci.yml`](../workflows/freebsd-ci.yml) on rdma-core and run the workflow.

## Bumping the image

Rebuild in **freebsd-builder**, tag a new release (e.g. `v15.1.1`), publish, then update `FREEBSD_IMAGE_URL` in `freebsd-ci.yml`.
