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
1. The _IaaS Account & Consolidated Bill Request_ form (NEED LINK) got me started with an AWS account
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
1. At this point, you will want to check your security group to ensure you have it configured per the figure below.  I arrived here through trail and error.

![Security Group Details](./BOSH_GPDB_Security_Group_Settings_Worked.png)


