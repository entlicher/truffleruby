# Releases Branches

Binary suites are needed to run the GraalVM gate.
Therefore, some care must be taken so they are deployed.

### If there is no release branch in GraalVM

Just make a PR against `master` as usual.
The latest commit from TruffleRuby master will be picked up when creating the
GraalVM release branch.

### If there is a release branch in GraalVM

When there is a release branch in GraalVM,
the fix's commits should be applied on top of the current TruffleRuby revision
in that GraalVM release branch, and not bring in all commits in `master` to reduce risks.

First create a release branch in TruffleRuby.
The name of the branch must start with `release` (for binary suites to be deployed).

```bash
cd truffleruby
git checkout -b release_<descriptiveSuffix> $TRUFFLERUBY_SHA1_IN_THE_GRAALVM_RELEASE_BRANCH
```

Then add fixes and when ready, create a PR (to master) on GitHub.
Use `jt pr` to trigger the CI.
`jt pr` will push the corresponding release branch to the internal repository,
which will deploy the binary suites of TruffleRuby.
This means at any time you can use the last commit revision of that branch to update the import in GraalVM.
You must still wait after `jt pr` for the deploy jobs to run and the binary suites to be deployed (a few minutes).

After the fixes are successfully integrated in the GraalVM release branch,
the PR __should be merged and not rebased__, so that `master` will contain the
fixes that go into the GraalVM release as well.
In case it was rebased, it is not a big problem but you need to push a branch
pointing to the last commit of the fixes to the internal repository
to keep the commit in GraalVM alive.

### Fallback: deploying binary suites manually

If for some reason, the steps above could not be followed, you can still deploy
binary suites for a given commit.

If the commit is already part of `master`, just go to the commit view and
trigger the 3 deploy jobs manually.

If the commit is not part of master, create a branch starting with `release`
and push it to the internal repository. This alone will deploy binary suites.
