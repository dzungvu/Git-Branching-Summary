# Git Flow

## Table of Contents
- [Before we begin](#1-before-we-begin)
    - [Version control](#11-version-control)
    - [What is git](#12-what-is-git)
- [Git strategy](#git-strategy)
    - [GitFlow](#git-flow)
    - [Github Flow](#github-flow)
    - [Gitlab Flow](#gitlab-flow)
    - [Trunk-based development](#trunk-based-development)
- [Choose the best git branching strategy](#how-to-choose-the-best-branching-strategy)
- [Our current strategy & the problems of it](#whats-our)

## 1. Before we begin

### 1.1 Version control
What is version control?

    Version control, also known as source control, is the practice of tracking and managing changes to software code.

Related declaration:

    Version control systems are software tools that help software teams manage changes to source code over time.

History of version control

<figure>
<img src="https://git-scm.com/book/en/v2/images/local.png" alt="local version control" style="width:100%">
<figcaption align = "center"><b>Fig.1 - Local Version Control Systems</b></figcaption>
&emsp;
&emsp;
<img src="https://git-scm.com/book/en/v2/images/centralized.png" alt="centralize version control" style="width:100%">
<figcaption align = "center"><b>Fig.2 - Centralized Version Control Systems</b></figcaption>
&emsp;
&emsp;
<img src="https://git-scm.com/book/en/v2/images/distributed.png" alt="distributed verson control" style="width:100%">
<figcaption align = "center"><b>Fig.3 - Distributed Version Control Systems</b></figcaption>
</figure>
&emsp;


### 1.2 What is Git

Git is a mature, actively maintained open source project originally developed in 2005 by Linus Torvalds ([REF](https://www.atlassian.com/git/tutorials/what-is-git>))

![Linus Torvalds](https://media.wired.com/photos/5b9ffc835031ce6b87548307/16:9/w_1760,h_990,c_limit/Linus-Linux-495992840.jpg)


**Git mindset**

```
Git thinks of its data more like a series of snapshots of a miniature filesystem. With Git, every time you commit, or save the state of your project, Git basically takes a picture of what all your files look like at that moment and stores a reference to that snapshot. To be efficient, if files have not changed, Git doesn’t store the file again, just a link to the previous identical file it has already stored. Git thinks about its data more like a stream of snapshots.
```
![Commit](https://git-scm.com/book/en/v2/images/deltas.png)
<br /><br /><br />
![File set](https://git-scm.com/book/en/v2/images/snapshots.png)

&emsp;

&emsp;

&emsp;

## **2. Git Strategy**

[comment]: <> (__________________________ )
[comment]: <> (|                         |)
[comment]: <> (|        Git Flow         |)
[comment]: <> (|_________________________|)

### **2.1 GitFlow**
> https://www.flagship.io/git-branching-strategies/

#### **2.1.1 What is it?**
This branching strategy consists of the following branches:

- Master
- Develop
- Feature: To develop new features that branches off the develop branch 
- Release: Help prepare a new production release; usually branched from the develop branch and must be merged back to both develop and master
- Hotfix: Also helps prepare for a release but unlike release branches, hotfix branches arise from a bug that has been discovered and must be resolved; it enables developers to keep working on their own changes on the develop branch while the bug is being fixed.

The main and develop branches are considered to be the main branches, with an infinite lifetime, while the rest are supporting branches that are meant to aid parallel development among developers, usually short-lived.

<details>
<summary>See more detail here</summary>
    
##### **How it works:**
- Develop and main branches

    Instead of a single main branch, this workflow uses two branches to record the history of the project. The main branch stores the official release history, and the develop branch serves as an integration branch for features. It's also convenient to tag all commits in the main branch with a version number.

    Develop branch will contain the complete history of the project, whereas main will contain an abridged version. Other developers should now clone the central repository and create a tracking branch for develop.

- Feature branches

    Each new feature should reside in its own branch, which can be pushed to the central repository for backup/collaboration. But, instead of branching off of main, feature branches use develop as their parent branch. When a feature is complete, it gets merged back into develop. Features should never interact directly with main.

    Feature branches are generally created off to the latest develop branch.

    When you’re done with the development work on the feature, the next step is to merge the feature_branch into develop.

- Release branches

    Once develop has acquired enough features for a release (or a predetermined release date is approaching), you fork a release branch off of develop. Creating this branch starts the next release cycle, so no new features can be added after this point—only bug fixes, documentation generation, and other release-oriented tasks should go in this branch. Once it's ready to ship, the release branch gets merged into main and tagged with a version number. In addition, it should be merged back into develop, which may have progressed since the release was initiated.

    Once the release is ready to ship, it will get merged it into main and develop, then the release branch will be deleted. It’s important to merge back into develop because critical updates may have been added to the release branch and they need to be accessible to new features. If your organization stresses code review, this would be an ideal place for a pull request.

    ![alt text](res/images/gitflow_release.svg "Title")
    <figcaption align = "center"><b>Git flow release</b></figcaption>

- Hotfix branches

    Maintenance or “hotfix” branches are used to quickly patch production releases. Hotfix branches are a lot like release branches and feature branches except they're based on main instead of develop. This is the only branch that should fork directly off of main. As soon as the fix is complete, it should be merged into both main and develop (or the current release branch), and main should be tagged with an updated version number.
    
    Having a dedicated line of development for bug fixes lets your team address issues without interrupting the rest of the workflow or waiting for the next release cycle. You can think of maintenance branches as ad hoc release branches that work directly with main

    ![alt text](res/images/gitflow_hotfix.svg "Title")
    <figcaption align = "center"><b>Git flow hotfix</b></figcaption>

- Summary

    The overall flow of Gitflow is:

    1. A develop branch is created from main
    2. A release branch is created from develop
    3. Feature branches are created from develop
    4. When a feature is complete it is merged into the develop branch
    5. When the release branch is done it is merged into develop and main
    6. If an issue in main is detected a hotfix branch is created from main
    7. Once the hotfix is complete it is merged to both develop and main

    
    
</details>
&emsp;

#### **2.1.2Visialize**

![alt text](res/images/gitflow.png "Title")
<figcaption align = "center"><b>Fig.4 - Git strategy 1: GitFlow</b></figcaption>

#### **2.1.3 Pros & cons**
Pros
- Allows for parallel development to protect the production
- The various types of branches make it easier for developers to organize their work

Cons
- As more branches are added, they may become difficult to manage as developers merge their changes from the development branch to the main
    <details>
        <summary>Explain more</summary>
        
        Developers will first need to create the release branch then make sure any final work is also merged back into the development branch and then that release branch will need to be merged into the main branch.
        
    </details>
    &emsp;

- In the event that changes are tested and the test fails, it would become increasingly difficult to figure out where the issue is exactly as developers are lost in a sea of commits.
- Indeed, due to GitFlow’s complexity, it could slow down the development process and release cycle. In that sense, GitFlow is not an efficient approach for teams wanting to implement continuous integration and continuous delivery.
&emsp;

&emsp;

[comment]: <> (__________________________ )
[comment]: <> (|                         |)
[comment]: <> (|       Github Flow       |)
[comment]: <> (|_________________________|)
### **2.2 Github Flow**
#### **2.2.1 What is it?**
GitHub Flow is a simpler alternative to GitFlow ideal for smaller teams as they don’t need to manage multiple versions.

<details>
    <summary>See more detail here</summary>
    
- Unlike GitFlow, this model doesn’t have release branches. You start off with the main branch then developers create branches, feature branches that stem directly from the master, to isolate their work which are then merged back into main. The feature branch is then deleted.

- The main idea behind this model is keeping the master code in a constant deployable state and hence can support continuous integration and continuous delivery processes.
    
</details>
&emsp;

#### **2.2.2 Visualize**
![alt text](res/images/githubflow.png "Title")
<figcaption align = "center"><b>Git strategy 2: Github Flow</b></figcaption>

&emsp;

#### **2.2.3 Pros & cons**
Pros:
- Github Flow focuses on Agile principles and so it is a fast and streamlined branching strategy with short production cycles and frequent releases.
- This strategy also allows for fast feedback loops so that teams can quickly identify issues and resolve them.
- Since there is no development branch as you are testing and automating changes to one branch which allows for quick and continuous deployment.
- This strategy is particularly suited for small teams and web applications and it is ideal when you need to maintain a single production version.

Cons:
- The lack of development branches makes this strategy more susceptible to bugs and so can lead to an unstable production code if branches are not properly tested before merging with the master-release preparation and bug fixes happen in this branch. The master branch, as a result, can become cluttered more easily as it serves as both a production and development branch.
- This model is more suited to small teams and hence, as teams grow merge conflicts can occur as everyone is merging to the same branch and there is a lack of transparency meaning developers cannot see what other developers are working on.

### **2.3 GitLab Flow**
#### **2.3.1 What is it?**
GitLab Flow is a simpler alternative to GitFlow that combines feature-driven development and feature branching with issue tracking.

While GitHub Flow assumes that you can deploy into production whenever you merge a feature branch into the master, GitLab Flow seeks to resolve that issue by allowing the code to pass through internal environments before it reaches production, as seen in the image below.

#### **2.3.2 Visualize**
![alt text](res/images/gitlabflow.png "Title")
<figcaption align = "center"><b>Fig.4 - Git strategy 1: Trunk-based development</b></figcaption>

&emsp;

[comment]: <> (__________________________ )
[comment]: <> (|                         |)
[comment]: <> (| Trunk-based development |)
[comment]: <> (|_________________________|)

### **2.4 Trunk-based development**

#### **2.4.1 What is it?**

Trunk-based development is a branching strategy that in fact requires no branches but instead, developers integrate their changes into a shared trunk at least once a day. This shared trunk should be ready for release anytime.

The main idea behind this strategy is that developers make smaller changes more frequently and thus the goal is to limit long-lasting branches and avoid merge conflicts as all developers work on the same branch. In other words, developers commit directly into the trunk without the use of branches.


Trunk-based development (TBD) is a branching model for software development where developers merge every new feature, bug fix, or other code change to one central branch in the version control system. This branch is called “trunk”, “mainline”, or in Git, the “master branch”.


> https://www.split.io/glossary/trunk-based-development/

&emsp;

#### **2.4.2 Visualize**

![alt text](res/images/trunk-based-development-branching-strategy.png "Title")
<figcaption align = "center"><b>Git strategy 4: Trunk-based development (Scale)</b></figcaption>

#### **2.4.3 Pros & cons**
Pros:
- It enhances collaboration as developers have better visibility over what changes other developers are making as commits are made directly into the trunk without the need for branches.
- Trunk-based development paves the way for continuous integration as the trunk is kept constantly updated.

Cons:
- Because trunk-based development does not require branches, this eliminates the stress of long-lived branches and hence, merge conflicts or the so-called ‘merge hell’ as developers are pushing small changes much more often. This also makes it easier to resolve any conflicts that may arise.

&emsp;

## **How to choose the best branching strategy**

Great for open-source projects that require strict access control to changes. This is especially important as open-source projects allow anyone to contribute and so with Git Flow, you can check what is being introduced into the source code.

However, GitFlow, as previously mentioned, is not suitable when wanting to implement a DevOps environment. In this case, the other strategies discussed are a better fit for an Agile DevOps process and to support your CI and CD pipeline.

| Product type and its release method | Team size | Collaboration maturity | Applicable mainstream branch mode |
| ----------- | ----------- | ----------- | ----------- |
| All | Small team | High | Trunk-Based Development (TBD)|
| Products that support continuous deployment and release, such as SaaS products | Middle | Moderate | GitHub-Flow and TBD|
| Products with a definite release window and a periodic version release cadence, such as iOS apps | Middle | Moderate | Git-Flow and GitLab-Flow with release branch |
| Products that are demanding for product quality and support continuous deployment and release, such as basic platform products | Middle | Moderate | GitLab-Flow |
| Products that are demanding for product quality and have a long maintenance cycle for released versions, such as 2B basic platform products | Large | Moderate | Git-Flow |



## **3. What's our?**

### **3.1 Use case**

#### **3.1.1 Normal case with hot fix**
![alt text](res/images/ourstrategy.png "Our strategy")
<figcaption align = "center"><b>Our strategy</b></figcaption>

- If any member have a merge request, they first need to merge the central branch into their own branch, resolve all the merge conflick; then create merge request.

- All branch (features & bugs) are merged into the central line (`develop`). The a specific build version will be built here
- After a specific built version has been released, a new branch will be created(5.0.25(1257)).

&emsp;

#### **3.1.2 Normal case with hot fix**
![alt text](res/images/ourstrategywithhotfix.png "Our strategy")
<figcaption align = "center"><b>Fig.4 - Our strategy with hotfix</b></figcaption>

- From the current released branch, create a new branch to implement the hotfix, merge the hotfix back to the released branch. Build and release the hotfix. Then merge the released branch into central branch to take the hotfix code.

&emsp;

#### **3.1.3 Abnormal case**
![alt text](res/images/ourstrategywithpickfeature.png "Our strategy")
<figcaption align = "center"><b>Fig.4 - Our strategy with hotfix</b></figcaption>

- Create a new branch and add all the code from the feature need to be picked. Then merge it into the new release build.

&emsp;

### **3.2 What is the disadvantaged?**
`
We can not pick a new feature from the branch that implemented that feature because this branch now have the code of other branch (Since the develop branch was merged back into the feature branch)
`


<font size="5"> HOW OUR COMPANY GOING </font>

- Have a plan to release but it's just a plan (Don't put trust on it).
- Define a list of features for the next release. But the deadline can be delay and the next release can become a the next small release (Not all the feature in the release note be used to publish).

![alt text](res/images/ourhard1.png "Title")
<figcaption align = "center"><b>Our hard 1</b></figcaption>

Plan: The next release rl.2 contains:
- fea.1.rl.2
- fea.2.rl.2

Change request: rl.2 only contain fea.2.rl.2

Ask: <font size = "7"> How? </font>

> Cherry-pick all commits in fea.2.rl.2?

> Don't merge the fea.1.rl.2 into develop until the next release comes?


### **3.3 What's others?**
![alt text](res/images/strategy1.png "Title")
<figcaption align = "center"><b>Git strategy 5: Trunk-based development Scale (Custom)</b></figcaption>

#### **Note:**

master is the main branch. Contain all newest fixes and features.

The branch M120 is the production branch. Mean that if any release version of master branch can go live to production then the branch Mxxx can be started there.

With hot fix:

Hot fix will take the started point from the master branch. Where the Mxxx branch start. Then when the fix is finished. It will be merged into master branch. Then use cherry-pick to pick the fix code into the Mxxx branch.



[comment]: <> (Create demo for this strategy)



![alt text](res/images/cherry-pick.png "Title")

#### ***Cherry-pick definition***

`git cherry-pick` is a powerful command that enables arbitrary Git commits to be picked by reference and appended to the current working HEAD. Cherry picking is the act of picking a commit from a branch and applying it to another. `git cherry-pick` can be useful for undoing changes. For example, say a commit is accidently made to the wrong branch. You can switch to the correct branch and cherry-pick the commit to where it should belong.

> https://www.atlassian.com/git/tutorials/cherry-pick

&emsp;
## How to use `git cherry pick`
&emsp;

To demonstrate how to use `git cherry-pick` let us assume we have a repository with the following branch state:

    a - b - c - d   Main
         \
           e - f - g Feature

`git cherry-pick` usage is straight forward and can be executed like:

```
git cherry-pick commitSha
```

In this example `commitSha` is a commit reference. You can find a commit reference by using git log. In this example we have constructed lets say we wanted to use commit `f` in main. First we ensure that we are working on the main branch.
```
git checkout main
```
Then we execute the cherry-pick with the following command:
```
git cherry-pick f
```
Once executed our Git history will look like:

    a - b - c - d - f   Main
         \
           e - f - g Feature
The f commit has been successfully picked into the main branch




## *Other topic*
1. Forking workflow (How Github Fork work)
