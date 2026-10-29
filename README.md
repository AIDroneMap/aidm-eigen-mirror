# Eigen mirror for AIDM builds

This is an unmodified mirror of https://gitlab.com/libeigen/eigen for
[AIDM](https://github.com/AIDroneMap/aidm) builds. Eigen is licensed under
MPL-2.0 as upstream. The upstream COPYING files and file-level notices
remain in every mirrored source tree, including older releases with
additional licences.

Upstream branches and tags keep their names and commit IDs. GitLab
merge-request refs are excluded. This separate `aidm-mirror-docs` default
branch contains only these maintenance notes. It does not change any
upstream source branch or tag.

The repository is archived to keep it read-only. Issues, wiki, projects
and GitHub Actions are disabled. There is no scheduled sync. Refresh it
only when an AIDM Eigen pin changes.

## Pins used by AIDM

| Use | Commit | Upstream ref containing it |
| --- | --- | --- |
| OpenMVS and OpenPointClass | `7176ae16238ded7fb5ed30a7f5215825b3abd134` | branch `3.4`, tag `3.4.1` |
| Draco 383 submodule | `3147391d946bb4b6c68edd901f2add6ac1f31f8c` | tag `3.4.0` |
| mvs-texturing Eigen 3.3.2 | `477d1e81928a8514d2cca6a6e1e7e478eecd8634` | tag `3.3.2` |

## Refresh when a pin changes

A repository administrator temporarily unarchives this repository. In a
fresh directory, fetch only upstream branches and tags:

```sh
git clone --bare https://gitlab.com/libeigen/eigen.git eigen.git
git --git-dir=eigen.git remote add aidm https://github.com/AIDroneMap/aidm-eigen-mirror.git
git --git-dir=eigen.git push aidm 'refs/heads/*:refs/heads/*' 'refs/tags/*:refs/tags/*'
```

For each new full commit ID, check that a branch or tag contains it:

```sh
git --git-dir=eigen.git cat-file -e "$EIGEN_PIN^{commit}"
git --git-dir=eigen.git for-each-ref --contains="$EIGEN_PIN" refs/heads refs/tags
```

If no ref contains a required commit, fetch that commit from upstream,
then add and push a tag named after its short SHA:

```sh
git --git-dir=eigen.git fetch origin "$EIGEN_PIN"
short=$(git --git-dir=eigen.git rev-parse --short=12 "$EIGEN_PIN")
git --git-dir=eigen.git tag "$short" "$EIGEN_PIN"
git --git-dir=eigen.git push aidm "refs/tags/$short:refs/tags/$short"
```

Verify the mirrored commit IDs from GitHub. Update the table on this docs
branch with a signed-off commit. Keep all upstream source commits intact.
Archive the repository again before merging the AIDM pin change. Validate
that change with GitLab blocked, including recursive submodule fetches.
