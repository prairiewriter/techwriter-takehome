### How do you revert a change from one branch and add it to another?

**Scenario:** You make an update and commit to the `master` branch. Afterward, you realize that you should've made this change to another branch named `staging`. What do you do?

**Answer**

1. Find the commit hash on `master`:

   ```
   git log --oneline -n 3
   ```

2. Apply the commit to `staging` with cherry-pick:

   ```
   git checkout staging
   git cherry-pick <commit-hash>
   ```

3. Remove the commit from `master`:

   - If it has not been pushed:

     ```
     git checkout master
     git reset --hard HEAD~1
     ```

   - If the commit has been pushed, revert it:

     ```
     git checkout master
     git revert <commit-hash>
     git push
     ```

### How do you squash git commits?

**Scenario:** You have a GitHub branch with 5 separate commits. What is the command line sequence to squash those 5 commits into a single commit?

**Answer**

1. Check out the branch and start a rebase of the five commits:

   ```
   git checkout my-branch
   git rebase -i HEAD~5
   ```

2. Keep `pick` on the first (oldest) commit and change `pick` to `squash` (or `s`) on the other four commits.

3. Save and close, then edit the combined commit message when prompted. If you already pushed the branch, update it with:

   ```
   git push --force-with-lease
   ```

### What is wrong with this sample JSON syntax?

```
{
  "first_name" : "Franz",
  "last_name" : "Kafka",
  "location" : "San Francisco"
  "streams" : true,
  "kafka" : 2.3 
}
```

**Answer**

There is a missing comma after `"location" : "San Francisco"` before `"streams"`.

### What is the Kafka broker port?

**Scenario:** Complete step 1 of [this Confluent Platform quick start](https://docs.confluent.io/platform/current/get-started/platform-quickstart.html) to install a local version of Confluent Platform. After completing step 1, what port number is the broker running on?

**Answer**

The broker is running on port 9092.

**Notes**

- **Docker must be running first.** Step 1 doesn't say to start Docker Desktop (or confirm the Docker engine is running) before `docker compose up -d`. If it isn't running, the command fails with "error during connect: this error may indicate that the docker daemon is not running." I recommend adding a prerequisite check, such as running `docker info`, before step 5.

- **No time estimate for the first run.** The first `docker compose up -d` downloads 11 container images. On my connection this took about 9 minutes (the slowest image took roughly 520 seconds), and Control Center needed additional time to become available afterward. I recommend adding a note that the first run can take 10 minutes or more, depending on connection speed, so readers don't assume the command has stalled.

- **Minor:** steps 2 and 4 both say `cd cp-all-in-one` (because of the nested directory), and step 3 doesn't explain how to choose a branch or whether it's optional. Adding a short explanation to each step might reduce confusion.

- **Where to run the commands isn't stated.** The steps don't say which terminal to use (for example, PowerShell on Windows) or that `docker compose up -d` must be run from the nested `cp-all-in-one` directory that contains `docker-compose.yml`. A one-line instruction might help readers on different operating systems.

- **An undocumented confirmation dialog appeared on Windows.** After running `docker compose up -d`, a dialog appeared asking to confirm I wanted to allow Confluent to run on my computer. The quick start doesn't mention this. If it's expected on Windows, a note should be added saying what the prompt is, how to respond, and whether declining it affects the stack.

- **Some containers exited after startup.** After `docker compose up -d`, `docker compose ps` showed only 9 of the 11 services in the docs' sample output. Running `docker compose ps -a` showed that `schema-registry` and `rest-proxy` had exited (code 1). The `schema-registry` log said it couldn't find the broker (`Expected 1 brokers but found only 0`), which suggests it started before the broker was ready. Restarting both containers with `docker compose restart` brought all 11 services to Up. The docs mention restarting a component that isn't Up, but they don't mention that `docker compose ps` hides exited containers or why this might happen. A short troubleshooting note would help.

### What are the default topics created when you install Confluent Platform using the quick start?

**Scenario:** After you complete step 1 of [this quick start](https://docs.confluent.io/platform/current/get-started/platform-quickstart.html), open Control Center at http://localhost:9021/. Note that Control Center takes a few minutes to load in your browser. What are the names of the default topics that are created by Kafka?

**Answer**

With internal topics hidden (the Control Center default), the following four topics appear:

- `default_ksql_processing_log`
- `docker-connect-configs`
- `docker-connect-offsets`
- `docker-connect-status`

When the **Hide internal topics** toggle is off, there are 15 topics listed.

### What is the name of the Connect cluster that is created?

**Scenario:** In Control Center, click on **Connect** in the left-hand navigation. What is the name of the Connect cluster that displays?

**Answer**

The Connect cluster is named **`connect-default`**.