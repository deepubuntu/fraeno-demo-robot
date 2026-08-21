# Fraeno demo robot

Try Fraeno without robot hardware or an existing robotics repository. This
template contains a small ROS 2 Humble robot with a sensor driver, controller,
health service, movement action, diagnostics, and transform.

You will open two pull requests:

1. A harmless change that Fraeno passes.
2. A simulated driver update that still builds but stops sensor data from
   reaching the controller. Fraeno blocks it.

The complete trial runs in GitHub Actions. It does not connect to physical
hardware.

## What you need

- a GitHub account
- access to the Fraeno private beta
- GitHub Actions enabled on your copy of this repository

Fraeno currently supports ROS 2 Humble on Ubuntu 22.04 and `amd64`.

## 1. Create your copy

Click **Use this template**, choose **Create a new repository**, and create the
repository under your GitHub account. A public repository is easiest for this
trial.

You can also [fork this repository](https://github.com/deepubuntu/fraeno-demo-robot/fork).
If GitHub pauses Actions on the fork, open its **Actions** tab and enable them.

Clone your new repository. Replace `YOUR-GITHUB-NAME` below:

```bash
git clone https://github.com/YOUR-GITHUB-NAME/fraeno-demo-robot.git
cd fraeno-demo-robot
```

## 2. Request access and install Fraeno

1. [Request private-beta access](https://fraeno.com/#access). Include the
   GitHub username that owns your copy.
2. [Install the Fraeno GitHub App](https://github.com/apps/fraeno-robotics) on
   only your demo repository.
3. Wait for your installation to be approved. Before approval, Fraeno reports
   a neutral **Fraeno is in private beta** check and does not run the robot.

## 3. Pin the Fraeno runner

In your repository, open **Settings**, then **Secrets and variables**,
**Actions**, and **Variables**. Create this repository variable:

```text
Name
FRAENO_RUNNER_IMAGE

Value
us-central1-docker.pkg.dev/fraeno-prod/fraeno-runner/runner@sha256:399a573b5b81d8baf3570f491c7958cc15b4ffeecadd760c0906ef8d7825c8d9
```

The image is public and pinned to the Fraeno `v0.2.4` release.

## 4. Watch a safe update pass

Create a documentation-only pull request:

```bash
git switch -c fraeno/safe-update
printf '\nFraeno safe-update trial completed.\n' >> README.md
git add README.md
git commit -m "Try a safe robot update"
git push --set-upstream origin fraeno/safe-update
```

Open the pull request on GitHub. The **Fraeno / robot integration** check runs
the trusted robot and the candidate robot. Both behave the same, so the check
passes. Merge or close the pull request before continuing.

## 5. Watch a dangerous update get blocked

Return to the default branch and create a second pull request:

```bash
git switch main
git pull --ff-only
git switch -c fraeno/dangerous-update
printf '2.0.0\n' > fraeno-fixture-version.txt
git add fraeno-fixture-version.txt
git commit -m "Simulate a dangerous sensor-driver update"
git push --set-upstream origin fraeno/dangerous-update
```

Open the pull request on GitHub. Version `2.0.0` changes the sensor publisher
from reliable delivery to best effort. The project still builds, but the
controller stops receiving sensor readings and `/robot/command` falls silent.
Fraeno detects the changed behavior and blocks the pull request.

Do not merge the dangerous update. Close its pull request when you finish.

## What the result means

A passing check means the behaviors declared in [`.fraeno.yml`](.fraeno.yml)
did not regress in this virtual test. It does not prove that every possible
physical behavior is safe.

For your own ROS 2 repository, follow the
[complete onboarding guide](https://github.com/deepubuntu/fraeno/blob/main/docs/onboarding.md).

See a real [safe external trial pass](https://github.com/Thabhelo/fraeno-demo-trial/actions/runs/32513196936)
and a [dangerous external update blocked](https://github.com/Thabhelo/fraeno-demo-trial/actions/runs/32513414015).
