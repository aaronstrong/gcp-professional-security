# [Migration to the Cloud](https://cloud.google.com/architecture/migration-to-gcp-getting-started)

**Table of contents:**
- [Migration to Google Cloud](#ddos-mitigation)
        - [Lift and Shift](#cloud-armor)
        - [Improve and Move](#cloud-security-scanner)
        - [Remove and replace]()
    - [Cloud Adoption Framework]()
    - [The Migration Path]()
- [Disaster Recovery Planning Guide]()
    - [Key DR Metrics]()
    - [DR Patterns]()
- [Backup and Recovery]()



## Migration to Google Cloud

A public cloud environment has the advantage that you don't have to manage the whole resource stack by yourself. You can focus on the aspect of the stack that is most valuable to you.
| Resources                  | On-prem<br>environment | Private hosting<br>environment | Public cloud<br>environment |
|----------------------------|------------------------|--------------------------------|-----------------------------|
| Physical security & safety | You                    | Service Provider               | Service Provider            |
| Power and network cabling  | You                    | Service Provider               | Service Provider            |
| Hardware                   | You                    | Depends on Service Provider    | Service Provider            |
| Virtualization Platform    | You                    | You                            | Service Provider            |
| App Resources              | You                    | You                            | You                         |

<b>Types of Migration</b>

- Lift and Shift
- Improve and Move
- Remove and replace (aka Rip and Replace)


### Lift and Shift

In a lift and shift migration, you move workloads from a source environment to a target environment with minor or no modifications or refactoring. The modifications you apply to the workloads to migrate are only the minimum changes you need to make in order for the workloads to operate in the target environment.

Ideal when a workload can operate as-is in the target environment, or when there is little or no business need for change. Requires the least amount of time because the amount of refactoring is kept to a minimum.

Lift and shift migrations are the easiest to perform because your team can continue to use the same set of tools and skills that they were using before. These migrations also support off-the-shelf software. 

On the other hand, the results of a lift and shift migration are non-cloud-native workloads running in the target environment. These workloads don't take full advantage of cloud platform features, such as horizontal scalability, fine-grained pricing, and highly managed services.

### Improve and Move

In an improve and move migration, you modernize the workload while migrating it. In this type of migration, you modify the workloads to take advantage of cloud-native capabilities, and not just to make them work in the new environment.

If the current architecture or infrastructure of an app isn't supported in the target environment as it is, a certain amount of refactoring is necessary to overcome these limits.

Another reason to choose the improve and move approach is when a major update to the workload is necessary in addition to the updates you need to make to migrate.

An improve and move migration also requires that you learn new skills.

### Remove and replace

In a remove and replace migration, you decommission an existing app and completely redesign and rewrite it as a cloud-native app.

Remove and replace migrations let your app take full advantage of Google Cloud features, such as horizontal scalability, highly managed services, and high availability. Because you're rewriting the app from scratch, you also remove the technical debt of the existing, legacy version.

A remove and replace migration also requires new skills. You need to use new toolchains to provision and configure the new environment and to deploy the app in that environment.

## Cloud Adoption Framework

The Google Cloud Adoption Framework serves both as a map for determining where your business information technology capabilities are now, and as a guide to where you want to be.

![](https://cloud.google.com/static/architecture/images/migration-to-gcp-getting-started-1-gcp-adoption-framework.svg)

The framework assesses four themes:

* **Learn**. The quality and scale of your learning programs.

* **Lead**. The extent to which your IT departments are supported by a mandate from leadership to migrate to Google Cloud.

* **Scale**. The extent to which you use cloud-native services, and how much operational automation you currently have in place.

* **Secure**. The capability to protect your current environment from unauthorized and inappropriate access.

For each theme, you should be in one of the following three phases, according to the framework:

* **Tactical**. There are no coherent plans covering all the individual workloads you have in place. You're mostly interested in a quick return on investments and little disruption to your IT organization.

* **Strategic**. There is a plan in place to develop individual workloads with an eye to future scaling needs. You're interested in the mid-term goal to streamline operations to be more efficient than they are today.

* **Transformational**. Cloud operations work smoothly, and you use data that you gather from those operations to improve your IT business. You're interested in the long-term goal of making the IT department one of the engines of innovation in your organization.

## The Migration Path

![](https://cloud.google.com/static/architecture/images/migration-to-gcp-getting-started-migration-path.svg)

There are four phases of your migration:

* **Assess**. In this phase, you perform a thorough assessment and discovery of your existing environment in order to understand your app and environment inventory, identify app dependencies and requirements, perform total cost of ownership calculations, and establish app performance benchmarks.

* **Plan**. In this phase, you create the basic cloud infrastructure for your workloads to live in and plan how you will move apps. This planning includes identity management, organization and project structure, networking, sorting your apps, and developing a prioritized migration strategy.

* **Deploy**. In this phase, you design, implement and execute a deployment process to move workloads to Google Cloud. You might also have to refine your cloud infrastructure to deal with new needs.

* **Optimize**. In this phase, you begin to take full advantage of cloud-native technologies and capabilities to expand your business's potential to things such as performance, scalability, disaster recovery, costs, training, as well as opening the doors to machine learning and artificial intelligence integrations for your app.


## [Disaster Recovery Planning Guide](https://cloud.google.com/architecture/dr-scenarios-planning-guide)

Plan for catastrophe

- Network outage
- Application update failure
- Hardware failure
- Natural Disaster
- Data deletion

### Key DR Metrics

> **Note**: Good article from Google on [Architecting disaster recovery for cloud infrastructure outages](https://cloud.google.com/architecture/disaster-recovery). Includes many GCP services.

![](https://cloud.google.com/static/architecture/images/rpo-rto.svg)

- RTO (recovery time objective)
    - Maximum acceptable length of time that your application can be offline
- RPO (recovery point objective)
    - Maximum acceptable lenght of time during which data might be lost from your application due to downtime

Typically, the smaller your RTO and RPO values are (that is, the faster your application must recover from an interruption), the more your application will cost to run. The following graph shows the ratio of cost to RTO/RPO.

![](https://cloud.google.com/static/architecture/images/dr-scenarios-planning-rto-rpo-cost-ratio.png)


#### DR Patterns

DR patterns are considered to be cold, warm, or hot. These patterns indicate how readily the system can recover when something goes wrong.

* **[Cold Recovery Example](https://cloud.google.com/architecture/dr-scenarios-for-applications#cold-pattern-recovery-to-gcp)** - In a cold pattern, you have minimal resources in the DR Google Cloud project—just enough to enable a recovery scenario.

![](https://cloud.google.com/static/architecture/images/dr-scenarios-for-apps-onprem-cold-pattern-state-1.svg)


* **[Warm Recovery Exmaple](https://cloud.google.com/architecture/warm-recoverable-static-site-failover-load-balancer)** - In this pattern, a load balancer directs traffic to Compute Engine instances in managed instance groups that serve the content. In an outage, you update the external HTTP(S) load balancer configuration and fail over to a static site in Cloud Storage.

![](https://cloud.google.com/static/architecture/images/warm-web-server-overview-load-balancer.svg)

* **[Host Recovery Example](https://cloud.google.com/architecture/dr-scenarios-for-applications#hot_ha_web_application)** - A hot pattern when your production environment is running on Google Cloud is to establish a well-architected HA deployment. This scenario takes advantage of HA features in Google Cloud—you don't have to initiate any failover steps, because they will occur automatically in the event of a disaster.

![](https://cloud.google.com/static/architecture/images/dr-scenarios-for-apps-gcp-hot-pattern.svg)

## Backup and Recovery

As of May 2, 2023 - Google has a service account Google Cloud Backup and DR that can include Compute Engine VMs, databases, file systems, VMware VMs, and general applications.

![](https://cloud.google.com/static/backup-disaster-recovery/docs/images/concepts/Backup-dr_1.jpg)

