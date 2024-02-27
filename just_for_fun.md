```bash
λ docker pull alpine
Using default tag: latest
latest: Pulling from library/alpine
bca4290a9639: Pull complete 
Digest: sha256:c5b1261d6d3e43071626931fc004f70149baeba2c8ec672bd4f27761f8e1ad6b
Status: Downloaded newer image for alpine:latest
docker.io/library/alpine:latest

What's Next?
  View a summary of image vulnerabilities and recommendations → docker scout quickview alpine

λ docker scout quickview alpine
    i New version 1.5.0 available (installed version is 1.4.1) at https://github.com/docker/scout-cli
    ✓ SBOM of image already cached, 19 packages indexed

  Target   │  alpine:latest  │    0C     0H     0M     0L   
    digest │  ace17d5d883e   │                              

What's Next?
  Include policy results in your quickview by supplying an organization → docker scout quickview alpine --org <organization>
```
* SBOM = Software Bill of Materials
