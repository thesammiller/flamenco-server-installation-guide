# Flamenco Server Installation Guide

This documentation attempts to provide full, step-by-step details for installing the components necessary to host your own Flamenco Server. There are many use-cases for self-hosting a Flamenco server. However, for most people, the services provided by Blender Cloud should be sufficient. As a bonus, subscriptions to Blender Cloud supports Blender. If you do not need a personal Flamenco server, please consider the more convenient solution and help the Blender Foundation in the process.

Setting up everything from scratch is hard. Even if you want to go 100% self-hosted, the Blender developers recommend getting a bit more experience with Flamenco using the Blender Cloud server first.

Installing a Flamenco Server requires the coordination of a few different installations, including the Blender Cloud website and the Blender ID log-in system. The process can be complicated. There are some applications which this document assumes familiarity with:
- Linux terminal - Bash, Ubuntu’s service/systemctl and developer tools like Git    
- Docker - used for MongoDB, Redis and RabbitMQ    
- Python - Pillar is built on Flask, Blender ID on Django and Poetry installs dependencies    
- Databases - Blender ID uses MySQL, Blender Cloud uses MongoDB & Redis   

The following instructions have been pieced together from a variety of sources.    
- [Pillar](https://pillarframework.org/development/install/)    
- [Blender Cloud](https://developer.blender.org/diffusion/BC/)    
- [Blender Cloud Docker Files](https://developer.blender.org/diffusion/BC/browse/master/docker/)    
- [Blender ID](https://docs.blender.org/id/) 
- [Blender ID Development Setup](https://git.blender.org/gitweb/gitweb.cgi/blender-id.git/blob/HEAD:/docs/docs/development_setup.md)


