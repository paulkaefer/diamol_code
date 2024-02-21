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

### Discussion notes
* "Benefits of a VM without the downside" says Yong.

## Chapter 2: Understanding Docker and running Hello World
* VMs: own OS; can use GB of RAM and lots of CPU
  * isolation at the cost of density
  * licensing costs; burden of OS updates
* Containers: share OS of computer running; makes them extremely lightweight
  * can have typically 5-10x as many as VMs on the same hardware
  * density, but apps are in own containers, so you also get isolation --> Docker is efficient!

# Once complete:
- [ ] consider a certification
