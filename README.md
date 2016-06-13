# bosh-gpdb-lessons-learned

This is my attempt to preserve what I learned in my attempt to get the GPDB BOSH release
working, including getting the Director running.

## Quick Overview
- It was not easy or intuitive for me
- Each time I attempted `bosh-init deploy ./bosh.yml`, it failed with the same error: `Post https://mbus:mbus-password@52.204.81.118:6868/agent: dial tcp 52.204.81.118:6868: i/o timeout`
- It did, eventually, work, and I was able to use this in a demo in Reston, VA on June 9, 2016
- The above would not have been possible without the helpful assistance of Todd Ritchie, via
  the #big-data-services Slack channel
- It is not possible to deploy GPDB using _BOSH lite_ due to its lack of support for _Power DNS_
- I am a huge fan of the differentiating power of cloud agnosticism, which BOSH provides

## Resources
1. The _IaaS Account & Consolidated Bill Request_ form got me started with an AWS account.  This is accessible via the [PA Wiki](https://sites.google.com/a/pivotal.io/pa-wiki/tools)
2. The [GPDB BOSH release code](https://s3.amazonaws.com/bds-ci/gpdb-bosh-release/greenplum-0.11.4-artifacts.tgz)
3. The _README.md_ contained within the above archive
4. [Deploying ... with BOSH](https://docs.pivotal.io/partners/deploying-with-bosh.html): this link appears within the README.md
5. [Initializing BOSH environment on AWS](http://bosh.io/docs/init-aws.html), which is linked from item 4
6. The [bosh.yml](./bosh.yml) I used to deploy the BOSH Director
7. A [diff](./mods_greenplum-0.11.4.txt) showing the mods I made within the greenplum-0.11.4-artifacts.tgz material
8. A [dump](./bash_history_bosh_cli_node.txt) of Bash history from the Ubuntu Trusty node I deployed into EC2 and ran all of this on

## These Steps Were Run from a Mac Laptop
1. Install AWS CLI tools: `pip install awscli`
1. Configure this CLI, based on details of your AWS account (just hit ENTER to preserve existing values):

    ```
    [airmike:example-tile-docs-resources]$ aws configure
    AWS Access Key ID [****************ZDDA]:
    AWS Secret Access Key [****************ciqx]:
    Default region name [us-east]: us-east-1
    Default output format [json]:
    ```

1. Refer to _Step 1: Prepare an Environment_, [here](https://docs.pivotal.io/partners/deploying-with-bosh.html), and perform 1 through 6, but *change* _ExampleKeyPair_ to _bosh_ throughout, since a later phase will assume the existence of a key pair called _bosh_.
1. At this point, you will want to check your security group to ensure you have it configured per the figure below.  `sg-00cb477b` is the security group resulting from the actions carried out above.
![Security Group Details](./BOSH_GPDB_Security_Group_Settings_Worked.png)
1. I also found it useful to go into the _VPC Dashboard_ and modify the subnet for my VPC so that it would automatically assign a public IP to any VM deployed there (see below).
![Auto assign public IP](./VPC_Dashboard_Auto_Assign_Public_IP.png)
1. The next part is to launch an Ubuntu Trusty VM.  I did this using the _Vagrant bosh-lite_ setup, but the result was I had the AMI _ami-ffa94192_ running on an _m3.xlarge_ VM, in the subnet created earlier.  For now, I will omit the details on this, but can add them later if this is useful.  I used the same key pair, _bosh_, for this VM.
1. SSH into this VM: `ssh -i ./bosh.pem ubuntu@52.91.102.63` (replace _52.91.102.63_ with your public IP)

## The Following Steps Were Run from the VM, via the SSH Connection
1. Refer to my [Bash history](./bash_history_bosh_cli_node.txt) if you need clarification
1. Install _bosh-init_:

    ```bash
    curl -O https://s3.amazonaws.com/bosh-init-artifacts/bosh-init-0.0.92-linux-amd64
    chmod +x ./bosh-init-0.0.92-linux-amd64
    ln -s ./bosh-init-0.0.92-linux-amd64 ./bosh-init
    ```

1. Edit the [BOSH deployment manifest](./bosh.yml) to suit your environment.  Look for `<--- Replace`
1. Deploy the BOSH Director: `./bosh-init deploy ./bosh.yml`
1. Install BOSH CLI: `sudo gem install bosh_cli`
1. Target this BOSH Director: `bosh target 192.168.1.6`.  Then you are prompted to enter a user name and password, use `admin` for each of them.
1. Download, extract GPDB BOSH release: `curl https://s3.amazonaws.com/bds-ci/gpdb-bosh-release/greenplum-0.11.4-artifacts.tgz | tar xzvf -`
1. Change into the new directory: `cd greenplum-0.11.4/`
1. Refer to the _README.md_ there, starting with step 2 (though, you already performed step 5).  You may find it useful to also refer to [the diff showing my mods](./mods_greenplum-0.11.4.txt) and that [Bash history](./bash_history_bosh_cli_node.txt).

## Caveats
* I have tried to avoid duplicating all the steps outlined within references cited here, but it could be that having a single, concise (and correct, though I am not the authority) reference, is simpler than one containing multiple links to external resources.
* It has been a few days since I walked through all of this, so I need to attempt it again, following these steps, and update this document to correct any inaccuracies or omissions.


