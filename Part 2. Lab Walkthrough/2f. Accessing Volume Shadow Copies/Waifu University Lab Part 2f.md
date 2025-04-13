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

#### Section 2g. Looking Around the Network
With reference to the lab, the next activity the threat actor did was open a file that was supposed to be in a hidden share from the beachhead host. There's a brief explainer of what a hidden share is [here](https://superuser.com/questions/309361/what-are-windows-hidden-shares-good-for). After a lot of OSINTing, there's a possible event code associated to this hidden network share [here](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/default.aspx). And its codes typically begin with "514", provided the OS version is Windows 10+ . 

A suggested way to narrow out the lab logs included adding the following columns below, and the starting point being the point in time the Volume Shadow copy was created. 

![image](images/29_cutoff_logs_before_network_share.jpg)

However, there were a lot of logs with "514*", and so, one other supporting column that is handy to include is "winlog.event_data.ShareName". A brief explainer of that can be found [here](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/file-access-auditing.html). Amongst the values seen in ShareName, one interesting one was "SuperSecretSecureShare". And for a few of them, when the event.code is 5145 (meaning a network share object was checked to see whether client can be granted desired access), and any files associated with it (with the RelativeTargetName column). 

![image](images/30_filename_to_find.jpg)



