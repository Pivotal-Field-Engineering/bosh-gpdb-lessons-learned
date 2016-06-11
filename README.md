# bosh-gpdb-lessons-learned

This is my attempt to preserve what I learned in my attempt to get the GPDB BOSH release
working, including getting the Director running.

## Quick Overview
- It was not easy or intuitive for me
- It did, eventually, work, and I was able to use this in a demo in Reston, VA on June 9, 2016
- The above would not have been possible without the helpful assistance of Todd Ritchie, via
  the #big-data-services Slack channel
- I am a huge fan, believe in the cloud agnosticism BOSH can provide

## Resources
1. The _IaaS Account & Consolidated Bill Request_ form (NEED LINK) got me started with an AWS account
2. The [GPDB BOSH release code](https://s3.amazonaws.com/bds-ci/gpdb-bosh-release/greenplum-0.11.4-artifacts.tgz)
3. The _README.md_ contained within the above archive
4. The [bosh.yml](./bosh.yml) I used to deploy the BOSH Director
5. A [diff](./mods_greenplum-0.11.4.txt) showing the mods I made within the greenplum-0.11.4-artifacts.tgz material
6. A [dump](./bash_history_bosh_cli_node.txt) of Bash history from the Ubuntu Trusty node I deployed into EC2 and ran all of this on


