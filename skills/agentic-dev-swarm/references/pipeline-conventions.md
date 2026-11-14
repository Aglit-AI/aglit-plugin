# Aglit Pipeline conventions

Give each independently deployed package one `.aglit/pipelines.yaml`. Put staging in the Pipeline that owns the deployment. When the Public API deploys the staging website and Console together, keep that deployment under Public API. Keep Web and Desktop responsible for their own artifacts and release commands.

- Name stages **Build**, **Staging**, and **Production**. Add platform qualifiers when one Pipeline displays multiple platforms, such as **Build (Mac)** and **Staging (Windows)**. Keep stage IDs stable when renaming labels because gates and run history use IDs.
- Use `parallel` for independent platform lanes, such as `mac` and `windows`. Stages within a lane run in order by default. An ordinary stage after the parallel group waits for every lane. Keep a release in its own lane when it only needs that lane's build.
- Put checks under the stage that produced the candidate. Use short names such as **Verify readiness** and **Browser verification**. Sibling workflows run in parallel. Put checks that must run in order inside a single command or model them as sequential stages. Close a staging lease after the required verification, without a separate **Accept Staging** promotion stage.
- Start Production disabled with explicit approval and a dependency on the build outputs it consumes. Staging success must never enable Production. In version 1, an explicit `needs` override belongs on an initially disabled terminal stage and refers to earlier IDs in the same lane. Include any build workflow IDs that produce required artifacts.
- Reuse the configured Pipeline cache and verified artifacts for the admitted source, platform, and build inputs. Build once where environment configuration can be applied at deployment. If staging and production have different signed app identities, share source and compatible caches and package each identity separately.
- Show the employee from the registered assignment or recorded attempt. Omit the employee label when there is no recorded name.

A single-platform example. The commands refer to scripts in the owning package.

```yaml
version: 1
name: Public API
cache:
  directory: ~/.aglit-ai/pipelines/public-api
stages:
  - id: build
    name: Build
    command: build
    runner:
      platform: darwin
  - id: staging
    name: Staging
    command: deploy:staging
    workflows:
      - id: readiness
        name: Verify readiness
        command: deploy:verify-staging
  - id: production
    name: Production
    command: deploy
    needs: [build]
    enabledByDefault: false
    manualApproval: true
```

Keep shared development on the team's development branch, such as `dev-swarm`. Register the integrated branch, such as `master` or `main`, for the continuous Pipeline. By default, a relevant integrated change queues a run. Run any explicitly configured development-branch qualification using staging identities and artifacts. A queued run keeps its frozen source and definition.

When a merge appears missing, compare the merged commit, the controller's selected source, the build result, the staging deploy result, and the revision served by staging. A successful build only proves that the artifact was built. If a lock blocks staging, identify the original run and confirm its closure before using the supported recovery procedure. Keep the run history and Production approval intact.
