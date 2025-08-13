<!-- # devops ci-cd_pipeine_project_aws -->

# 🚀 7-DevOps-Project Over 7 Days Upskilling 


* 🔗 **Documentation:**

  * [Documentation](https://mega.nz/file/WzJy1ZLD#2pJ6mQiEElLUnKCu-flPkcffPyC5ak58fCLaydw4Kjw) – Compiled Documentations from 7-Days-DevOps Project Series.
 

<!-- Blog Article – uncomment & replace when published 
- **Blog:** ICON *7-Days DevOps Project Series - A HandsOn Practical Implementation Guide using AWS Native Solution*  
  <https://dev.to/suvrajeet/mastering-aws-iam-how-to-control-ec2-access-like-a-pro-part-5-4gd0>
-->


In this 7-day AWS DevOps challenge, I built a full end-to-end CI/CD pipeline for a Java web application. Each day focused on a different step: from setting up the web app on EC2 to automating builds and deployments. Along the way I gained hands-on experience with AWS services (EC2, S3, IAM, CodeBuild, CodeDeploy, CodePipeline, CodeArtifact, etc.) and tools like VS Code, Git/GitHub, and Maven. The following sections walk through each day’s **steps performed** and **key learnings**. (Original project references are linked at the end of each section.)
<!-- 
* 🔗 **Original Projects:**

  * [Project 1: Set Up a Web App in the Cloud (Day 1)](https://learn.nextwork.org/projects/aws-devops-vscode?track=high) – Launch an EC2 instance and scaffold a Java web app.
  * [Project 2: Connect a GitHub Repo with AWS (Day 2)](https://learn.nextwork.org/projects/aws-devops-github?utm_source=project) – Install Git, create a GitHub repo, and push code.
  * [Project 3: Secure Packages with CodeArtifact (Day 3)](https://learn.nextwork.org/projects/aws-devops-codeartifact-updated?track=high) – Set up AWS CodeArtifact for dependency management.
  * [Project 4: Continuous Integration with CodeBuild (Day 4)](https://learn.nextwork.org/projects/aws-devops-codebuild-updated?track=high) – Automate the build process with AWS CodeBuild.
  * [Project 5: Deploy a Web App with CodeDeploy (Day 5)](https://learn.nextwork.org/projects/aws-devops-codedeploy-updated?track=high) – Automate deployment to EC2 with CodeDeploy.
  * [Project 7: Build a CI/CD Pipeline with AWS CodePipeline (Day 7)](https://learn.nextwork.org/projects/aws-devops-codepipeline-updated?track=high) – Orchestrate the entire pipeline end-to-end.
-->

## Day 1: Set Up a Web App in the Cloud with VSCode 🛠️

**Steps Performed:**

* 🔧 **Launched an AWS EC2 instance** (Amazon Linux) and connected via SSH from my local machine.
* 📥 **Installed Java (Amazon Corretto 8) and Apache Maven** on the EC2 instance. This provided the Java runtime and build tools needed for a web app.
* 📦 **Generated a basic Java web app** using Maven’s archetype (e.g. `mvn archetype:generate ... maven-archetype-webapp`). Maven scaffolds the project structure automatically.
* 🖥 **Connected VS Code to the EC2 instance** using the Remote – SSH extension. This let me open and edit the web app files as if they were local.

**Key Takeaways:**

* Learned to configure a remote dev environment: installing packages and using VSCode Remote SSH.
* Understood how Maven archetypes can auto-generate a project template, saving setup time.
* Gained confidence navigating AWS EC2, Linux commands, and setting environment variables (e.g. `JAVA_HOME`).
* **Skills covered:** AWS EC2, Linux CLI, Java/Apache Maven, VSCode Remote SSH, basic Java web project structure.

## Day 2: Connect a GitHub Repo with AWS 🔗

**Steps Performed:**

* 💻 **Installed Git** on the EC2 instance (`sudo dnf install git -y`). Verified with `git --version`.
* 🌐 **Created a GitHub repository** (e.g. *devops-web-project*). Added a descriptive README.
* 📂 **Initialized Git in my project folder** on the EC2 instance (`git init`) and **added a remote** pointing to the GitHub repo.
* 🔀 **Committed and pushed the code:** Added all files (`git add .`), committed (`git commit -m "Initial web app"`), and pushed to GitHub (`git push -u origin master`). Set up a GitHub personal access token when prompted.

**Key Takeaways:**

* Experienced first-hand how Git tracks code changes and enables collaboration.
* Learned the flow of connecting a local repo to GitHub: `git init`, `git remote add`, commit, and push.
* Understood the importance of using a **personal access token** for HTTPS pushes (improves security).
* **Skills covered:** Git version control, GitHub repo setup, SSH/HTTPS access to Git, writing Git commit messages, using Git in a remote Linux environment.

## Day 3: Secure Packages with AWS CodeArtifact 📦

**Steps Performed:**

* 🆕 **Created an AWS CodeArtifact repository** (e.g. *devops-cicd*) via the AWS Console. Enabled Maven Central as an upstream so public libraries can be retrieved.
* ⚙️ **Configured the repository domain and permissions:** Created an IAM policy allowing `codeartifact:GetAuthorizationToken`, `GetRepositoryEndpoint`, and `ReadFromRepository`.
* 🤝 **Attached the IAM policy to an IAM role for EC2**, then associated that role with my EC2 instance. This gave the instance permissions to fetch packages.
* 🔑 **Set up Maven to use CodeArtifact:** Used the AWS Console’s “view connection instructions” to generate a temporary auth token and added the CodeArtifact `<server>`, `<profile>`, and `<mirror>` XML snippets into a `settings.xml` file in my project.
* 📥 **Compiled the project via CodeArtifact:** Ran `mvn -s settings.xml compile`. Maven fetched dependencies through CodeArtifact (testing the integration).

**Key Takeaways:**

* Learned how CodeArtifact centralizes and secures project dependencies.
* Gained experience creating and managing **IAM policies and roles** to grant services access to CodeArtifact.
* Understood the process of retrieving a CodeArtifact token and updating Maven settings to point to the new repository.
* **Skills covered:** AWS CodeArtifact repository setup, IAM policy/role creation, Maven settings (XML), dependency management in CI/CD.

## Day 4: Continuous Integration with AWS CodeBuild ⚙️

**Steps Performed:**

* 🗂 **Created an S3 bucket** to hold build artifacts (e.g. compiled JAR/WAR files).
* 🛠 **Configured an AWS CodeBuild project:** Linked it to the GitHub repo as source.
* 📜 **Authored a `buildspec.yml`** in the project root, defining build commands (e.g. `mvn package`) and artifact paths.
* 🔐 **Set up IAM permissions:** Assigned a service role to CodeBuild with access to the S3 bucket and other resources.
* ▶️ **Triggered and monitored the build:** Launched a build; CodeBuild pulled the code, ran the build commands, and stored the artifact in S3. Debugged minor errors (e.g. environment variables).

**Key Takeaways:**

* Automated the compile/test stage of CI: every code push can trigger a CodeBuild run.
* Learned the structure of **`buildspec.yml`** to define phases (install, pre-build, build, post-build) and artifacts.
* Understood best practices: storing build outputs in S3 and using IAM roles to secure access.
* **Skills covered:** AWS CodeBuild setup, writing buildspec.yml, S3 artifact storage, CI pipeline execution.

## Day 5: Automated Deployment with AWS CodeDeploy 🚀

**Steps Performed:**

* 🗄️ **Wrote deployment scripts:** Created bash scripts to install, start, and stop the web app on the EC2 instance (e.g. deploying the WAR file to Tomcat).
* 📄 **Created an `appspec.yml` file:** This file specified how CodeDeploy should run the scripts (e.g. hooks like `BeforeInstall`, `AfterInstall`, `ApplicationStart`).
* 🛠 **Set up CodeDeploy application and deployment group:** Defined a new CodeDeploy application and linked it to the EC2 instance (using tags or an Auto Scaling group).
* 🚀 **Deployed the app with one click:** Triggered a deployment; CodeDeploy pulled the latest build artifact from S3/GitHub and executed the specified scripts in order.
* 📝 **Verified the web app:** Checked that the application was running correctly on the EC2 public URL after deployment.

**Key Takeaways:**

* Transformed manual deployment steps into an automated workflow using CodeDeploy. As Evelyn Hale notes, *“Created an `appspec.yml` file, transforming my manual scripts into a structured CodeDeploy workflow… Created a CodeDeploy application and deployment group, turning my scripts into a fully automated, one-click deployment process.”*.
* Learned how CodeDeploy uses `appspec.yml` to manage application lifecycle events.
* Gained experience with AWS tagging/roles to associate EC2 instances with CodeDeploy.
* **Skills covered:** AWS CodeDeploy setup, appspec.yml structure, bash scripting for deployment, automated deployment pipelines.

## Day 6: *(Optional – Infrastructure as Code)* ⚖️

> *Note: While Day 6 often covers Infrastructure-as-Code (e.g. AWS CloudFormation to provision resources), the provided links focus on DevOps tools. However, I ensured that my EC2 instance and related resources were consistently configured (some projects used CloudFormation templates, others manual console steps). The key takeaway is understanding that IaC can automate environment setup.*

## Day 7: Build a CI/CD Pipeline with AWS CodePipeline 🔄

**Steps Performed:**

* 🎯 **Created an AWS CodePipeline:** This pipeline chained all prior steps: Source (GitHub) → Build (CodeBuild) → Deploy (CodeDeploy).
* 📦 **Integrated services:** Configured CodePipeline to use CodeBuild for compiling the app and CodeDeploy for deploying the compiled artifact. Linked the S3 bucket from Day 4 to store artifacts between stages.
* ☁️ **Used CloudFormation for infra:** (Optional) I used a CloudFormation template to ensure the EC2 instance, IAM roles, and S3 bucket were consistently recreated when needed.
* ▶️ **Ran the pipeline:** Pushed a change to GitHub and watched CodePipeline automatically trigger: it performed the CodeBuild build and then deployed via CodeDeploy without further input.
* 📈 **Monitored the workflow:** Verified each stage in the AWS Console, ensuring no errors (fixed minor IAM permission issues).

**Key Takeaways:**

* Achieved full end-to-end automation: as Shadrack Kalukwo summarizes, *“I used a suite of AWS services to build a CI/CD pipeline that automatically builds, tests, and deploys my web application whenever I push code changes to GitHub”*.
* Learned how AWS CodePipeline orchestrates multiple services (CodeBuild, CodeDeploy) into a single workflow.
* Cemented understanding of how each component (S3 for artifacts, EC2 for hosting, IAM for permissions, CloudFormation for infrastructure, GitHub for code) fits into the CI/CD picture.
* **Skills covered:** AWS CodePipeline configuration, end-to-end DevOps workflow, AWS CloudFormation basics, pipeline monitoring and debugging.

## Final Outcomes & Skills Gained 🎓

* **CI/CD Fundamentals:** Built a complete pipeline from source code commit to automated deployment.
* **AWS DevOps Toolset:** Hands-on experience with EC2, S3, IAM, CodeArtifact, CodeBuild, CodeDeploy, and CodePipeline.
* **Programming Toolchain:** Used Java/Apache Tomcat, Apache Maven, and VS Code for development and build.
* **Version Control:** Solidified Git/GitHub proficiency for team collaboration and source tracking.
* **Automation & Scripting:** Wrote shell scripts (`bash`) for server setup and learned to author YAML manifests (`buildspec.yml`, `appspec.yml`).
* **Infrastructure as Code:** Touched on CloudFormation for repeatable environment setup.
* **Problem-Solving:** Debugged CI/CD errors (build failures, permission issues) to ensure a smooth pipeline.

Each day’s practical tasks have made these concepts concrete. By **day 7**, I had a documented, working CI/CD pipeline. The original project guides were invaluable (see links above) for step-by-step instructions and best practices. This portfolio-style walk-through not only demonstrates the **steps performed** each day but also highlights my **learning outcomes**: a deep, practical grasp of AWS DevOps automation.

<!-- **Sources:** Technical details and steps are based on the official challenge guides and community write-ups, which document the AWS services and procedures used at each stage of the 7-day challenge. -->









---

## 🙏 Acknowledgments
This learning journey was powered & supported by NextWork's structured approach to cloud education, which made breaking down complex concepts into digestable-byte-sized-hands-on practicals accessible through systematic skill building & clear-actionable steps.

This write-up is based on - NextWork's - 7 Day DevOps Challenge!

Give it a try: <https://learn.nextwork.org/projects/aws-devops-cicd>

---









### Complete DevOps CI/CD Pipeline Demonstration on AWS - A HandsOn Project [NW]
