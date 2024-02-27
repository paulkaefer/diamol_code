# Setup/teardown
See `book_code.md` and `cleanup.md`, respectively.

# Preface
* author started working with Docker in 2014

# Part 1. Understanding Docker containers and images

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
> **go-offline**: This is the specific goal within the Dependency Plugin. When you execute this goal, Maven will download all the dependencies required for your project and store them in your local Maven repository. Going "offline" means that Maven will attempt to download all the necessary dependencies without connecting to the internet during subsequent builds, making your build process more self-contained.
> 
> In summary, when you run mvn -B dependency:go-offline, you are telling Maven to resolve and download all the dependencies for your project and store them locally, allowing you to build your project even when you don't have an internet connection. This is particularly useful in scenarios where you want to ensure that your build process can run independently of external network availability.

# Once complete:
- [ ] consider a certification
