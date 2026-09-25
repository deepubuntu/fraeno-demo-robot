# Fraeno demo robot

Try a fixed Fraeno demonstration without robot hardware, production engine
access, or an existing robotics repository. This template contains a small ROS
2 Humble robot fixture and a deliberately limited public check.

You will open two pull requests:

1. A harmless documentation change that passes.
2. A known fixture change that represents broken sensor delivery and is blocked.

The public demonstration recognizes only this repository and three built-in
fixture versions. It does not contain Fraeno's proprietary validation engine,
run arbitrary robot commands, or validate customer repositories.

## 1. Create your copy

Click **Use this template**, choose **Create a new repository**, and create the
repository under your GitHub account. A public repository is easiest for this
trial.

You can also [fork this repository](https://github.com/deepubuntu/fraeno-demo-robot/fork).
If GitHub pauses Actions on the fork, open its **Actions** tab and enable them.

Clone your new repository and replace `YOUR-GITHUB-NAME` below:

```bash
git clone https://github.com/YOUR-GITHUB-NAME/fraeno-demo-robot.git
cd fraeno-demo-robot
```

No Fraeno App installation, private runner image, repository variable, or beta
approval is required for this fixed demonstration.

## 2. Watch a harmless change pass

Create a documentation-only pull request:

```bash
git switch -c fraeno/safe-demo
printf '\nFraeno safe demo completed.\n' >> README.md
git add README.md
git commit -m "Try the safe Fraeno demo"
git push --set-upstream origin fraeno/safe-demo
```

Open the pull request on GitHub. The **Fraeno fixed public demo** check compares
the two recognized fixture versions and passes because the robot behavior is
unchanged. Merge or close the pull request before continuing.

## 3. Watch the known regression get blocked

Return to the default branch and create a second pull request:

```bash
git switch main
git pull --ff-only
git switch -c fraeno/dangerous-demo
printf '2.0.0\n' > fraeno-fixture-version.txt
git add fraeno-fixture-version.txt
git commit -m "Simulate the known sensor delivery regression"
git push --set-upstream origin fraeno/dangerous-demo
```

Open the pull request. Version `2.0.0` maps to the fixed demonstration where the
sensor publisher becomes best effort, the reliable controller receives no
sensor data, robot commands stop, and diagnostics report an error. The check
blocks the pull request.

Do not merge the dangerous demonstration. Close its pull request when finished.

## What this demonstrates

The demonstration shows the product interaction and a truthful historical
Fraeno pass/block scenario. It does not execute the production engine, inspect
arbitrary repositories, or certify physical safety.

To evaluate Fraeno on a real ROS 2 repository, request a guided private trial at
[fraeno.com](https://fraeno.com/#access).
