KEYSTATIC_GITHUB_CLIENT_ID=Iv23liF2vqvT7uYuWz59
KEYSTATIC_GITHUB_CLIENT_SECRET=b4f82c126a45e0c1ff0891d5c86fbb420ab5d683
KEYSTATIC_SECRET=355209e1403cdff7dcb785611369e28b659d14a88c7d2768f7735b7d8b1608c0f33f9796b355fab3
PUBLIC_KEYSTATIC_GITHUB_APP_SLUG=afriqsiliconltd-website-keystatic


The production support team of xFusionCorp Industries is working on developing some bash scripts to automate different day to day tasks. One is to create a bash script for taking websites backup. They have a static website running on App Server 1 in Stratos Datacenter, and they need to create a bash script named blog_backup.sh which should accomplish the following tasks. (Also remember to place the script under /scripts directory on App Server 1).


a. Create a zip archive named xfusioncorp_blog.zip of /var/www/html/blog directory.

b. Save the archive in /backup/ on App Server 1. This is a temporary storage, as backups from this location will be clean on weekly basis. Therefore, we also need to save this backup archive on Nautilus Backup Server.

c. Copy the created archive to Nautilus Backup Server server in /backup/ location.

d. Please make sure script won't ask for password while copying the archive file. Additionally, the respective server user (for example, tony in case of App Server 1) must be able to run it.

---
The Nautilus team doesn't want its data to be accessed by any of the other groups/teams due to security reasons and want their data to be strictly accessed by the devops group of the team.


Setup a collaborative directory /devops/data on app server 1 in Stratos Datacenter.

The directory should be group owned by the group devops and the group should own the files inside the directory. The directory should be read/write/execute to the user and group owners, and others should not have any access.

---