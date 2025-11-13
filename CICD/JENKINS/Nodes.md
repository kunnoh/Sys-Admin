# Slave Nodes
To add slave nodes in Jenkins, add SSH build agent, SSH Credentials plugins.   

**Manage Jenkins > Plugins > Available plugins**. Add 
**Manage Jenkins > Credentials > (global)->add credentials**
**Manage Jenkins > Nodes > + New Node**
**Manage Jenkins > System > SSH remote hosts#SSH sites->Add**

Create new item, to install java on the slave node, specifying each server on the ssh script.  
