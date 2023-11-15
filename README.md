### Prerequisites:
- Change Packages must be stored under a folder below the branch, such as the name of the system where the change package was created or another grouping name, which is defined using the Directory Template field in the OIEP delivery configuration using 'Change Package Git Delivery' method (e.g. '$systemname$/$changepackageid$'). This folder label is defined by '$systemname$' in this example, and will be used in the `REPO_PATH` variable.
- To use workflows, the user must create public/private key pairs, which are also used to connect via SFTP from a local computer, using Open SSH format, where the key starts with '-----BEGIN OPENSSH PRIVATE KEY-----' and ends with '-----END OPENSSH PRIVATE KEY-----'. The remote server must have permissions to connect and upload using these credentials. It is recommended to setup a local scp/sftp client on your computer to confirm access to the sftp and the ability to write a file before setting up this automation.
- For the workflow 'Send Change Package on Change', update the branch if you are using something other than "main" and set the 'paths:' value, which is the folder that is monitored for changes within the repository. Prior to running the action, update this code, replacing `stibo-user` with the value that REPO_PATH resolves to, followed by '/**'):
  ```
  on:
  push:
    branches: [ "main" ]
    paths:
      - 'stibo-user/**'
  ```


### The Following Action Secrets and Variables must be Defined
Go to `Settings -> Secrets and Variables -> Actions`

#### Secrets:
- `REMOTE_HOST` - URL of the SFTP host AKA Host Name. For Stibo SaaS systems, this is in the format of '[yourSystemName]-sftp.mdm.stibosystems.com'.
- `USERNAME` - User name that is used to connect to the remote host with priviledges to upload. For Stibo SaaS systems, this is the Username (DNS-1123 compliant) configured in the Accounts area of the SFTP Access Control screen.
- `KEY` - SSH private key of user in Open SSH format, which is related to the public key configured on the SFTP Account of the Username on Stibo SaaS systems.

The secrets above are used by the external action: https://github.com/wangyucode/sftp-upload-action and there is more information available about additional parameters. These can be applied only to job `SFTP uploader`, if desired.

#### Variables:
- `REPO_PATH` - the path below the Branch of the repository containing change packages, followed by a forward slash. In this example `stibo-user/` (not including quotes) is the value for the Repository variable.
- `SFTP_PATH` - the path on the application server where the hotfolder on the IIEP is configured to receive files.  