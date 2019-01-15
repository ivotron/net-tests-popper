# `cloudlab-multisite-transfer`

This pipeline will test multi-site transfers on the CloudLab 
infrastructure. The pipeline consists of the following stages:

  * [`setup`](./setup.sh). Allocate two nodes on two sites of the 
    [CloudLab](https://cloudlab.us) federated testbed. This is 
    automated by using 
    [`geni-lib`](https://bitbucket.org/barnstorm/geni-lib).

  * [`run`](./run.sh). This will instantiate a server and a client, 
    each of them being executed on one of the sites. The test is 
    orchestrated using [Ansible](https://github.com/ansible) and the 
    tests are packaged in [Docker](https://www.docker.com/get-started) 
    containers.

  * [`post-process`](./post-process.sh). This stage post-process the 
    output of the test, generating a table of results that is read by 
    the next stage.

  * [`plot`](./plot.sh). This stage plots the results using Python 
    data analysis tools (numpy, pandas, seaborn, matplotlib, etc.). 
    The plots report on the transfer test results.

# Parameters

The parameters to the pipeline are specified in the `parameters.yml` 
file. The variables of the pipeline are the following:

  * `test`. The type of test. Available tests: 
    [`iperf`](https://iperf.fr/), 
    [`netperf`](https://hewlettpackard.github.io/netperf/) and 
    [`wdt`](https://github.com/facebook/wdt).
  * `size`. The size of the transfer.
  * `directory`. The directory being transferred (only available for 
    `wdt`)
  * `linux-image`. The CloudLab image being booted. Available images: 
    `16.04` and `18.04`.

# Obtaining the pipeline

To add this pipeline to your project using the
[`popper` CLI tool](https://github.com/systemslab/popper):

```bash
cd your-repo
popper add ivotron/net-tests-popper/cloudlab-multisite-transfer
```

# Running the pipeline

To run the pipeline using the
[`popper` CLI tool](https://github.com/systemslab/popper), first 
define the following environment variables:

  * `CLOUDLAB_USER`. Your user at CloudLab.
  * `CLOUDLAB_PASSWORD`. Your password for CloudLab.
  * `CLOUDLAB_PROJECT`. The name of the project your account belongs 
    to on CloudLab.
  * `CLOUDLAB_PUBKEY_PATH`. The path to your SSH key registered with 
    CloudLab.
  * `CLOUDLAB_CERT_PATH`. The path to the cloudlab.pem file downloaded 
    in step 2.
  * `SSHKEY`. The path to your private SSH key registered with 
    CloudLab. and then run the pipeline:

```bash
cd net-transfer-popper
popper run cloudlab-multisite-transfer
```
