# Azure DevOps

- **Learn** how Azure DevOps can provide a streamlined DevOps pipeline
- **Sign up** for a free Azure DevOps account
- **Create** a new project in Azure DevOps

Having a streamlined development and deployment strategy can:

- **Reduce** delays in delivering features and avoid cost overruns
- **Allow** teams to be more effecient with limited resources
- **Generate** detailed and accurate performance metrics

## DevOps definition

> DevOps eliminates Development and Operation working in silos
It creates multidisciplinary teams that work together with shared and efficient practices and tools


- **Agile planning**, Ensure a prioritized backlog of work is available for the team and facilitate management for work including user stories, bugs, and more
- **Continuous Integration (CI)**, The process of automating the build and testing of code every time a team member commits changes to version control
- **Continuous Delivery (CD)**, The process to build, test, configure, and deploy from a build to a production environment.
- **Monitoring**, Use telemetry to deliver information about an application’s performance and usage patterns to aid learning as we iterate.

Comparing _elite performers_ to _low performers_:
- do **46 times** more frequent code deployments
- have **2,555 times** faster lead time from commit to deploy
- benefit from **seven times** lower change failure rate
- recover from incidents **2,604 times** faster

Some important points:

- **DevOps improves software delivery**, which increases competitive advantages, This enables companies to experiment with increasing customer adoption and satisfaction. Leads to better organizational performance, and often higher profitability & market share. Agility allows companies to respond to competitive threats, and pivot more quickly to keep up with compliance and regulatory requirements
- **How you implement cloud infrastructure matters**, The cloud improves software delivery performance and teams that adopt essential cloud characteristics are **23 times** more likely to be high performers
- **Open-source software improves performance**, High performers are 1.75 times more likely to extensively use open-source software. They are also 1.5 times more likely to expand open-source usage in future
- **Outsourcing by function is rarely adopted by elite performers and hurts performance**, Outsourcing can save money and provide a flexible labor pool, but must be used in the correct areas. Low-performing teams are almost four times as likely to outsource whole functions (testing, operations, etc.) then their higher-performing counterparts
- **Continuous delivery technical practices drive high performance**, These include monitoring and observability, continuous testing, database change management, and integrating security earlier in the software development process

DevOps is NOT:
- DevOps is not a methodology
- DevOps is not a specific piece of software
- DevOps is not a quick-fix for an organization's challenges
- DevOps is not just a team or a job title (Although these are reasonably common in the industry)

## What is Azure DevOps?
DevOps is about people, process and products

### DevOps products
There are many well known and widely used tools used across the industry for helping achieve success using DevOps. Many of the popular tools are Open Source products as well as commercial products from some of the world's largest technology companies.

Microsoft's Azure DevOps family of products is a highly scalable, reliable option that is used by many organizations. Just within Microsoft itself, there are over 80,000 users of Azure DevOps.

### Products
It's a suite of Products
- **Azure Boards**, proven agile tools to `plan`, `track`, and `discuss` work across your teams
- **Azure Pipelines**, allows you to `build`, `test`, and `deploy` with `CI/CD` that works with any language, platform, and cloud. Connect to GitHub or any other Git provider and deploy continuously
- **Azure Repos**, provide unlimited, cloud-hosted private and public Git repos to build better code with pull requests and advanced file management
- **Azure Test plans**, help you ship with confidence by providing manual and exploratory testing tools
- **Azure artificats**, give you the ability to `create`, `host`, and `share` packages with your team. You can easily add artifacts to your CI/CD pipelines with a single click

## Create an Azure DevOps account
- **free**, Microsoft provides free Azure DevOps accounts for individuals, small teams and open source projects
- **paid**, Enterprises can also sign up for Azure DevOps accounts that can scale to thousands of team members

1 To create an account go to [https://dev.azure.com ](https://dev.azure.com ) and click `Start free`.
2 Sign in using your Microsoft Account, or if you do not have a Microsoft account, click Create One! and complete the steps to create a new Microsoft account

### Create a new organization
Once logged in there should be menu. In the bottom left there should be an option that should say `New organization`, like so:
![](/assets/Screen Shot 2019-02-16 at 15.50.20.png)

Once you've clicked a new organization, your new organization will be reachable on the followingURL:
> https://dev.azure.com/[Orgname]

### Create a new project
Projects in Azure DevOps are generally synonymous with applications in your organization. It is good practice to avoid entering numbers in the project name. For example, try not to name your project something like Payroll 2.0 or Payroll 2018. A better approach would be to name your project *Payroll and then 2.0 or 2018 are most likely just branches in your source code repository and iterations for your work items.

Click create new project, you should get a dialog looking like this. Try fill in the defaults, like so:
![](/assets/Screen Shot 2019-02-16 at 15.56.29.png)

Once you click `Create`, it should look something like this:
![](/assets/Screen Shot 2019-02-16 at 16.00.16.png)


