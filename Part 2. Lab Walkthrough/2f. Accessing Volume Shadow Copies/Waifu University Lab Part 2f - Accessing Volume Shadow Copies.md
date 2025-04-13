# Xintra's Waifu University Lab Walkthrough

Writing up a walkthrough to figuring out the incident at XINTRA's Waifu University. </br> This lab is an emulation of Alphv/BlackCat ransomware group.

#### Section 2f. Accessing Volume Shadow Copies
This part of the lab is a bit more specific, and it's referring to Volume Shadow Copies. As per MS, anything associated with Volume Shadow copies will have "vssadmin" as part of its commands. Therefore, trying the wildcard command again, we can narrow down all the CLI executions with "vssadmin" that are part of it. 
And referring back to the previous section, "Breaching the University", the compromised host of the Waifu Uni network is CC-JMP-01. 

To bear in mind, there's a good chance he can only execute this vssadmin command when the threat actor has admin level rights, so the "User" field will also be handy to check in the logs. More about vssadmin documentation is [avail here](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/vssadmin)
With these three in mind, let's see what logs are available in ELK. 

![image](images/28_create_a_vol_shadow.jpg)

Before a bunch of delete commands, there is one Create shadow command for the /C: executed. And it matches our beachhead host's device name as well. Expanding the ParentCommandLine field as well, there's a file that is visible that was responsible for running the "create shadow command" of the C: . 

Now, proceeding to the next part of the lab. 





