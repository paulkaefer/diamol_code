# Setup/teardown
See `book_code.md` and `cleanup.md`, respectively.

# Preface
* author started working with Docker in 2014

# Part 1: Understanding Docker containers and images

## Chapter 1: Before you begin
* links: http://mng.bz/04lW, http://mng.bz/K29E (note both have equivalents like https://shortener.manning.com/K29E)
* Docker: "cost benefits of PaaS with the portability benefits of IaaS"
* nontransportability (is that a word?) of lambda functions
  * Nuclio, OpenFaaS, or Fn Project, are all popular open source serverless frameworks
* Hyperledger
* "Operators may have a background in tools like Bash, Nagios, PowerShell, and System Center. Developers work in Make, Maven, NuGet, and MSBuild."
  * OK, haven't heard of Nagios, NuGet, and System Center; I know little about Maven.
* CALMS framework for implementing DevOps: Culture, Automation, Lean, Metrics, and Sharing
* Things he won't cover: `containerd`, `cgroups`, and `namespaces`. Does LFS cover all these? I'm kind of working my way backwards--or downwards, as it were!
  * nor the Windows Host Compute Service, for that matter
  * he recs *Docker in Action* (second edition) by Jeff Nickoloff and Stephen Kuenzli

One helpful image:
![](./attachments/Figure_1-2.jpg)

### Discussion notes
* "Benefits of a VM without the downside" says Yong.

## Chapter 2: Understanding Docker and running Hello World
* VMs: own OS; can use GB of RAM and lots of CPU
  * isolation at the cost of density
  * licensing costs; burden of OS updates
* Containers: share OS of computer running; makes them extremely lightweight
  * can have typically 5-10x as many as VMs on the same hardware
  * density, but apps are in own containers, so you also get isolation --> Docker is efficient!
* "You can try a new piece of software--say, Elasticsearch, or the latest version of SQL Server, or the Ghost blogging engine--with the same type of `docker container run` commands you’ve been using here."
* "...Universal Control Plane (UCP), a commercial product from the company behind Docker (https://docs.docker.com/ee/ucp/ --> archived [here](https://web.archive.org/web/20180721171151/https://docs.docker.com/ee/ucp/)). Portainer is another option, which is an open source project. Both UCP and Portainer run as containers themselves, so they’re easy to deploy and manage."
  * UCP is a graphical user interface for containers
* "containerd is an open source component overseen by the Cloud Native Computing Foundation, and the specification for running containers is open and public; it’s called the Open Container Initiative (OCI)."
* "Docker is by far the most popular and easy to use container platform, but it’s not the only one. You can confidently invest in containers without being concerned that you’re getting locked in to one vendor’s platform."

### Discussion notes
* We asked ChatGPT, "If a Docker instance doesn't have an OS, is it even possible to run a different OS in it?" and it answered, "Docker containers don't contain a full operating system; instead, they package only the necessary components and dependencies required to run a specific application. Docker containers share the host operating system's kernel and resources, which allows them to be lightweight and portable." & mentioned running a VM if we need a totally different OS. It also said, "Docker is designed to be used with containerized applications, and it's not meant for running full-fledged operating systems."

## Chapter 3: Building your own Docker images
* Docker images may be packaged with a default set of configuration values for the application, but you should be able to provide different configuration settings when you run a container.

## Chapter 4: Packaging applications from source code into Docker Images
* Regarding Figure 4.1 Everyone needs the same set of tools to build a software project.
  * a relevant problem experienced on a project at work just today! which is a funny contrast to other projects actively using Docker
![](./attachments/Figure_4-1.jpg)
* multi-stage Dockerfiles "have more than one `FROM` instruction, and the steps are laid out to get maximum benefit from Docker's image layer cache"
* does the `dependency:go-offline` mean that once this runs once, we no longer have to be connected? From [here](https://maven.apache.org/plugins/maven-dependency-plugin/go-offline-mojo.html), it seems to install all needed dependencies, so it would appear that way.
  * ChatGPT helped. I asked:
> explain like I'm a freshman in computer science (who knows how to write & compile Java code) what "mvn -B dependency:go-offline" means

...part of the answer:

> ...
> 
> * **go-offline**: This is the specific goal within the Dependency Plugin. When you execute this goal, Maven will download all the dependencies required for your project and store them in your local Maven repository. Going "offline" means that Maven will attempt to download all the necessary dependencies without connecting to the internet during subsequent builds, making your build process more self-contained.
> 
> In summary, when you run mvn -B dependency:go-offline, you are telling Maven to resolve and download all the dependencies for your project and store them locally, allowing you to build your project even when you don't have an internet connection. This is particularly useful in scenarios where you want to ensure that your build process can run independently of external network availability.
* "Docker itself is written in Go"
* "Multi-stage Dockerfiles make your project entirely portable. You might use Jenkins to build your apps right now, but you could try AppVeyor’s managed CI service or Azure DevOps without having to write any new pipeline code--they all support Docker, so your pipeline is just `docker image build`."

## Chapter 5: Sharing images with Docker Hub and other registries
* Instead of `docker.io/diamol/golang:latest` (URL listed early), I see `https://hub.docker.com/r/diamol/golang`. But maybe `docker.io` will work when pulling?
* "Layers are only physically uploaded to the registry if there isn’t an existing match for that layer's hash."
  * cool! but I also wonder if there have ever been collisions...
* Changed the steps slightly for bypassing restrictions on `registry.local:*`... Settings >> Resources >> Proxies & added under "bypass..."
* "use specific image tags for the base images in your own Dockerfiles"

### Discussion
```
λ docker images registry.local:5001/gallery/ui --format="{{ .Tag }}"
```
Outputs:
```
2
2.1
2.1.106
latest
```
### Discussion #2
* The concept of manifests was in the Lab solution, but not explained in the chapter.
  * it's basically metadata about an image: https://docs.docker.com/reference/cli/docker/manifest/
* We tried `docker manifest inspect diamol/golang` and looked at the output.
* local registry doesn't seem to work exactly the way the book presents it. that said, we searched the text for "5000" and it's only in chapters 3 (unrelated) and 5 (used as port). So we get the concepts & can move on!

## Chapter 6: Using Docker Volumes For Persistent Storage
* bind mounting to have any arbitrary folder path surfaced to your container (such as a network drive)
* From Section 6.5, "Every container has a single disk, which is a virtual disk that Docker pieces together from several sources. Docker calls this the union filesystem."

### Discussion notes
* clever the way the image layers are read-only, allowing for repeatability

# Part 2: Running distributed applications in containers
* "...use Docker and Docker Compose to define, run, and manage applications that run across multiple containers." --> CI pipeline; health checks/observability

## Chapter 7: Running multi-container apps with Docker Compose
* "Docker is ideally suited to running distributed applications--from n-tier monoliths to modern microservices."
* "[YAML] translates easily to JSON (which is the standard language for APIs)"
* Section 7.2 references the NASA APOD API app as an example: Java frontend (website), API in Go, and a Node log collector.
* Section 7.4 mentions [Sqlectron](https://sqlectron.github.io/).

### Section 7.5: Understanding the problem Docker Compose solves
* "Docker Compose is a very neat way of describing the setup for complex distributed apps in a small, clear file format. The Compose YAML file is effectively a deployment guide for your application, but it’s miles ahead of a guide written as a Word document."
* Docker Compose basically kicks things off, but that's where it ends.
* might want to look up Docker Swarm vs. Kubernetes

# Useful links
* Author's YouTube (link from Chapter 6 lab's README file): https://is.gd/z8qHga
* [Docker Compose Viz](https://github.com/pmsipilot/docker-compose-viz)

## Chapter 8: Supporting reliability with health checks and dependency checks
* advice: logic to ensure dependencies are met. but also fail-fast (sometimes better to have an exited container than a running (but failing) container)
* find the balance... health checks that work, but don't do too much work, as they need to run often

## Chapter 9: Adding observability with containerized monitoring
* exposing metrics from your app; use Prometheus to cllect them and Grafana for visualization
* custom metrics seem comparable to what we do with Datadog... but defined in JavaScript
* using PromQL --> again, similar to Datadog's query language
* links to [Google's Site Reliability Engineering book](http://mng.bz/EdZj)
  * "Their focus is on latency, traffic, errors, and saturation, which they call the 'golden signals.'"
* inspiring idea: summary dashboard(s) in Grafana & then display on a big screen in the office!

## Chapter 10: Running multiple environments with Docker Compose
* "Moving to Docker fixes that problem (drift) because every application is already packaged with its dependencies, but you still need the flexibility to support different behavior for different environments."
* Figure 10.4: Removing duplication with override files that add environment-specific settings:
![](./attachments/Figure_10-4.jpg)
* "Extension fields are custom definitions"... seems similar to terraform variables?

## Chapter 11: Building and testing applications with Docker and Docker Compose
* "In this chapter you’re going to learn how to do continuous integration (CI) with Docker."
* "There’s a huge choice of managed services you can choose from that all support Docker--you can mix and match GitHub with Azure DevOps and Docker Hub, or you could use GitLab, which provides an all-in-one solution. Or you can run your own CI infrastructure in Docker containers."

### Questions
- [ ] what exactly are .sock files?
- [ ] how do we apply digital signatures? (see figure 11.14 in section 11.5)

### Discussion notes
* Yong changed `"5000:5000"` to `"5001:5000"` in the docker-compose.yml file, and got it working.
* he did say the author specifies Jenkins version, but not all the dependencies work, so he had to make some modifications

# Part 3: Running at scale with a container orchestrator
* Docker Swarm - "a simple and powerful orchestrator built into Docker"

## Chapter 12: Understanding orchestration: Docker Swarm and Kubernetes
> "Services are first-class objects in Docker Swarm"... what does `first-class` mean in this context?

> I know it’s not good, but at least we got 12 chapters in before we hit the “this sucks on Windows” moment, and I don’t think there are any more coming.

* Good comparison of Docker Swarm vs. Kubernetes.

## Chapter 13: Deploying distributed applications as stacks in Docker Swarm
* the reality is you [should] interact with Docker Swarm via YAML, not individual/manual commands
* "Docker containers can access all the host machine’s CPU and memory if you don’t specify a limit"

# Once complete:
- [ ] does LinkedIn have a Docker quiz?
- [ ] consider pursuing a certification
- [ ] maybe go through http://mng.bz/EdZj (see link in Chapter 9)
