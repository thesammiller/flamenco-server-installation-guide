# flamenco-server-installation-guide
A step-by-step guide for installing the Flamenco Server for Blender



This documentation attempts to provide full, step-by-step details for installing the components necessary to host your own Flamenco Server. There are many use-cases for self-hosting a Flamenco server. However, for most people, the services provided by Blender Cloud should be sufficient. As a bonus, subscriptions to Blender Cloud supports Blender. If you do not need a personal Flamenco server, please consider the more convenient solution and help the Blender Foundation in the process.

*“Setting up everything from scratch is hard... Even if you want to go 100% self-hosted, I would recommend getting a bit more experience with Flamenco using the Blender Cloud server first.”* - Dr. Sybren


For those who still want to install a Flamenco Server, please consider a one-time donation to Blender. This document was made possible by the help and support of those employed by the Blender Foundation.  As a suggested donation, a month of Blender Cloud is around $12, a year is $100. Consider it a down-payment on technical support. 

You can donate [here](https://www.blender.org/foundation/donation-payment/).

Installing a Flamenco Server requires the coordination of a few different installations, including the Blender Cloud website and the Blender ID log-in system. While there are a number of technologies and frameworks involved, many are shared in common and fortunately most do not need direct interaction when installing. However, there are some applications which anyone attempting an installation should probably be at least familiar with using:
- Linux terminal - Bash, Ubuntu’s service/systemctl and developer tools like Git    
- Docker - used for MongoDB, Redis and RabbitMQ    
- Python - Pillar is built on Flask, Blender ID on Django and Poetry installs dependencies    
- SQL - Blender ID uses MySQL, Blender Cloud and Flamenco use MongoDB   

To begin, create a fresh Ubuntu installation. This can be done directly on hardware or on a virtual machine. The following instructions have been pieced together from a variety of sources.    
- [Pillar](https://pillarframework.org/development/install/)    
- [Blender Cloud](https://developer.blender.org/diffusion/BC/)    
- [Blender Cloud Docker Files](https://developer.blender.org/diffusion/BC/browse/master/docker/)    
- [Blender ID](https://docs.blender.org/id/)    

Additional resources can be found in the developer docs for each repository. 

When you are ready, continue to the next section to begin. 

