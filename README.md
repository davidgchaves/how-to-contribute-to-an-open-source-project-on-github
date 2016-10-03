# Notes on *How to Contribute to an Open Source Project on GitHub*

## Table of Contents

1. [`fork` the project](#fork-the-project)
2. [`clone` locally](#clone-locally)
3. [Sync data](#sync-data)
4. [Create the Pull Request `branch`](#create-the-pull-request-branch)
5. [Make changes locally](#make-changes-locally)
6. [`push` changes to your `fork`ed project](#push-changes-to-your-forked-project)
7. [Create your Pull Request on the Original Project](#create-your-pull-request-on-the-original-project)
8. [Make some new changes locally](#make-some-new-changes-locally)
9. [Execute the CI scripts locally and `push`](#execute-the-ci-scripts-locally-and-push)
10. [`rebase upstream/master` for a cleaner history and `f`orce `push`](#rebase-upstream-master-for-a-cleaner-history-and-force-push)
11. [`squash` `commit`s and `f`orce `push`](#squash-commits-and-force-update)
12. [Notes](#notes)
13. [Links](#links)

## <a id="fork-the-project">1. `fork` the project</a>

Simply click github's `fork` button

## <a id="clone-locally">2. `clone` locally</a>

Clone your `fork`ed copy, **NOT** the original.

```console
➜ git clone git@github.com:<MY_USER>/<MY_PROJECT>.git
```

## <a id="sync-data">3. Sync data</a>

1. Rename `origin` `remote` from your `fork` to `github`.
2. Add `upstream` `remote` for the original project.
3. `fetch` the original project.
4. Link your `fork`ed `master` branch with the original project `master` branch (`upstream/master`).

```console
➜ git remote rename origin github
➜ git remote add upstream git@github.com:<ORIGINAL_USER>/<ORIGINAL_PROJECT>.git
➜ git fetch upstream
➜ git branch --set-upstream-to=upstream/master master
```

## <a id="create-the-pull-request-branch">4. Create the Pull Request `branch`</a>

We used the completely arbitrary `pull-request` namespace.

```console
➜ git checkout -b pull-request/<MY_PR_BRANCH>
```

## <a id="make-changes-locally">5. Make changes locally</a>

```console
➜ git commit -m "WIP: Almost there"
```

## <a id="push-changes-to-your-forked-project">6. `push` changes to your `fork`ed project</a>

We can push directly:

```console
➜ git push github pull-request/<MY_PR_BRANCH>
```

...or even better, specify the `upstream` for our `pull-request/<MY_PR_BRANCH>` and `push`, then:

```console
➜ git push --set-upstream github pull-request/<MY_PR_BRANCH>
➜ git push
```

## <a id="create-your-pull-request-on-the-original-project">7. Create your Pull Request on the Original Project</a>

Easy way:

1. Push `Compare and pull request` button.
2. Write your message.
3. Push `Create pull request`.

Hard way:

1. Push `New pull request` button.
2. Click on the `compare across forks` link.
3. Change the `head fork` to your personal `fork`.
4. Change `compare` to the branch you just pushed (`pull-request/<MY_PR_BRANCH>`).
5. Write your message.
6. Push `Create pull request`.

## <a id="make-some-new-changes-locally">8. Make some new changes locally</a>

```console
➜ git commit -m "WIP: Is it done?"
```

## <a id="execute-the-ci-scripts-locally-and-push">9. Execute the CI scripts locally and `push`</a>

In case of `Travis` with this `.travis.yml`

```json
{
  "os": "osx",
  "language": "node_js",
  "sudo": false,
  "node_js": "node",
  "script": "npm run release"
}
```

simply run

```console
➜ npm run release
➜ git push
```

## <a id="rebase-upstream-master-for-a-cleaner-history-and-force-push">10. `rebase upstream/master` for a cleaner history and `f`orce `push`</a>

Apply:

1. original `upstream/master` commits first,
2. our `fork`ed `pull-request/<MY_PR_BRANCH>` `commit`s on top.

```console
➜ git fetch upstream
➜ git rebase upstream/master
➜ git push -f
```

## <a id="squash-commits-and-force-update">11. `squash` `commit`s and `f`orce `push`</a>

```console
➜ git rebase -i <SHA_OF_THE_LATEST_COMMIT_FROM_MASTER>
  (SQUASH THE COMMITS, NOW!)
➜ git push -f
```

## <a id="notes">12. Notes</a>

1. `--set-upstream` is equivalent to `-u`.
2. `f`orce `push`ing updates to a `pull request` **will not** notify to the project maintainer, or anyone else watching the `pull request`.
3. Remember to **always** `git fetch upstream` before `git push`.

## <a id="links">13. Links</a>

- [Egghead: How to Contribute to an Open Source Project on GitHub](https://egghead.io/courses/how-to-contribute-to-an-open-source-project-on-github)
- [What open source project should I contribute to?](https://medium.com/@kentcdodds/what-open-source-project-should-i-contribute-to-7d50ecfe1cb4#.dou36jj12): ...with a nice `debugger;` trick to read source code.
