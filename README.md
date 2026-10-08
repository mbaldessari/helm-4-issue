Small reproducer for a bug introduced in helm 4

Specifically it seems to have been introduced between v4.1.4 and v4.2.0.
Likely culprit is:

```
commit 00638773d1366dc962c785de3d297cf0279b9a0d (HEAD)
Author: Johannes Lohmer <jojo.dev@lohmer.com>
Date:   Sat Mar 28 20:59:31 2026 +0100

   fix(values): do not copy chart-default nils into coalesced values

   Only user-supplied nils should survive coalescing. Chart-default nils
   defaults, not just user overrides. This caused:

- %!s(<nil>) in templates using Bitnami common.secrets.key (#31919)
- pluck fallbacks returning nil instead of falling through to globals
     (#31971)

   Fixes #31919
   Fixes #31971

   Signed-off-by: Johannes Lohmer <jojo.dev@lohmer.com>

pkg/chart/common/util/coalesce.go      | 29 ++++++++++++++++++++++++++++-
pkg/cmd/testdata/output/issue-9027.txt | 10 ++++++++--
2 files changed, 36 insertions(+), 3 deletions(-)
```

Upstream issue seems to be helm/helm#32530 ("Keys with null values removed from values.yaml")
