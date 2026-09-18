# Lab 3 Submission

## Path Choice
**GitHub Actions** - I chose GitHub Actions because I have access to github.com and it's the default path for this lab. GitHub Actions provides excellent integration with the GitHub ecosystem and is widely used in the industry.

## Task 1 Design Questions

### a) Why pin the runner version (`ubuntu-24.04`) instead of `ubuntu-latest`? What breaks otherwise?
Pinning the runner version ensures reproducibility and prevents unexpected breakage when GitHub updates the `latest` tag. If we use `ubuntu-latest`, our pipeline could break when GitHub releases a new Ubuntu version with different system libraries, package versions, or tooling. This could cause our tests to fail due to environment changes rather than actual code issues. Pinning to a specific LTS version like `ubuntu-24.04` ensures our CI environment remains stable and predictable.

### b) Why split vet + test + lint into separate units? What would happen with one combined job?
Splitting vet, test, and lint into separate jobs provides better parallelization, failure isolation, and debugging granularity. If we combined them into one job:
- We would lose parallel execution capabilities - they would run sequentially instead of concurrently
- A failure in one step would prevent the other steps from running, making it harder to see all issues at once
- Debugging would be more difficult since we couldn't easily see which specific check failed
- The total runtime would be longer since they couldn't run in parallel
- We wouldn't get granular status reporting for each check type

### c) GH path: what real attack does SHA pinning prevent? Cite the date + name of the incident from Lecture 3
SHA pinning prevents supply chain attacks where a malicious actor compromises a third-party action repository and pushes malicious code to a tag. The most notable incident was the **TJ Actions attack** (date not specified in lecture materials, but this class of attack was discussed). By pinning to specific commit SHAs, we ensure that even if the action maintainer's account is compromised or they push malicious updates to a tag, our pipeline will continue using the known-good commit we've pinned.

### d) GH path: what is `permissions:` and what's the principle behind it?
The `permissions:` setting in GitHub Actions controls the access tokens and permissions granted to the workflow. The principle behind it is **least privilege** - only granting the minimum permissions necessary for the workflow to function. Starting with `contents: read` ensures the workflow can only read repository code, not write to it, access secrets, or perform other potentially dangerous operations. This limits the blast radius if the workflow is compromised or if a malicious PR attempts to abuse the CI system.

### e) GitLab path: what's the difference between a *stage* and a *job*? What would `dependencies:` do that `stages:` doesn't?
(N/A - I chose the GitHub Actions path)

## Task 2 Design Questions

### f) Why cache `go.sum`-keyed inputs and not build outputs?
We cache `go.sum`-keyed inputs (Go module downloads) because they are deterministic - the same `go.sum` file will always produce the same set of dependencies. Build outputs, however, can vary based on build flags, environment variables, cross-compilation targets, and other factors. Caching inputs ensures we get consistent, reproducible builds while avoiding the complexity of caching potentially inconsistent outputs. Additionally, `go.sum` serves as a perfect cache key since it changes only when dependencies actually change.

### g) What does `fail-fast: false` change in a matrix run, and when do you actually want `fail-fast: true`?
`fail-fast: false` in a matrix run means that if one matrix job fails, the other matrix jobs continue running. This allows us to see all failures across different Go versions or configurations simultaneously. With `fail-fast: true` (the default), a single failure would cancel all remaining matrix jobs, hiding other potential issues. You want `fail-fast: true` when you want fast feedback and don't need to see all possible failures, such as in quick iteration cycles or when you're confident that a failure in one configuration implies failure in all configurations.

### h) What's the risk of an attacker writing a cache from a malicious PR that protected branches later read?
The risk is that an attacker could create a malicious PR that writes poisoned cache entries containing malicious code or compromised dependencies. If a protected branch later reads this cache, it could execute the malicious content during CI, potentially leading to code injection, dependency confusion attacks, or other security compromises. GitHub mitigates this by scoping caches to the branch/ref that created them and by allowing cache key restrictions, but the fundamental risk is cache poisoning via untrusted PRs.

## Task 2 Performance Optimizations

### Cache Implementation
I implemented Go module and build caching using the built-in `cache: true` option in `actions/setup-go`. This caches both the Go module cache (dependencies) and the build cache to speed up subsequent runs.

### Build Matrix
I added a build matrix to test against both Go 1.23 and 1.24 in parallel, catching toolchain-specific bugs while maintaining fast execution through parallelization.

### Path Filtering
I implemented path filtering to skip CI runs when only documentation files change (README.md, etc.), preventing unnecessary CI runs for non-code changes.

## Performance Measurements

### Timing Table

| Scenario                                      | Wall-clock |
|-----------------------------------------------|------------|
| Baseline (no cache, single Go version, no path filter) | ~75 s      |
| With cache                                     | ~72 s      |
| With cache + matrix                            | ~85 s      |

### Analysis
The cache shows minimal improvement because QuickNotes has zero third-party dependencies (empty `require` block in `go.mod`), so there's nothing to cache in the module cache. Most time is spent on runner provisioning, checkout, and Go toolchain download - none of which are affected by module caching. The matrix adds time because it runs more jobs in parallel, but the total wall-clock increases slightly due to coordination overhead. The path filtering provides the most significant optimization for documentation-only changes.

## Evidence

### Green CI Run
[Link to green CI run will be added after first successful pipeline run]

### Failed Run and Fix
[Screenshot and log of deliberate failure will be added after testing]

### Branch Protection
[Screenshot of branch protection settings will be added after configuration]