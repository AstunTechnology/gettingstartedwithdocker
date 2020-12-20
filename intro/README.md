# What is Docker and what is it not

![Docker logo](../pics/docker-logo.png)

Docker (other containerisation tools are available) is a technology for delivering applications. It is not a virtualisation technology. While both containers and virtual machines (VMs) are ways of delivering an isolated environment in which to run an application, the way they approach this is fundamentally different.

One analogy is that virtual machines are like houses, whereas docker containers are like apartments. Houses (VMs) are fully self-contained, with everything they need to be lived in, such as plumbing, heating, electricity, a bedroom, bathroom etc. Apartments might have bedrooms and bathrooms, but they have shared infrastructure, which is administered within the wider apartment block, and you rent the size of apartment that you need, be that a penthouse or a studio flat.

![Containers vs VMs](../pics/containers.png)

*https://www.docker.com/resources/what-container*

Hosting all your applications in a virtual machine introduces potential dependency and performance issues, as applications may compete with each other (eg when you run a resource-intensive package on your computer and everything grinds to a halt), and isolating applications in their own VM is a duplcation of resources. With containerisation, you build containers that include only the resources you need, and use the underlying infrastructure from the host. Virtual machines are a full operating system. 

{% hint style='info' %}
Docker doesn't currently work with Windows applications, or anything with a GUI. This means that you can't easily get a container running Internet Explorer/Edge or anything backed by IIS, or indeed QGIS. You can run Docker on windows though.
{% endhint %}



