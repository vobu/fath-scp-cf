# run folding@home on SAP CP cf

In these troublesome COVID-19 pandemic times, computing power needs to be pooled for the good cause.  
This MTA app uses the docker image https://hub.docker.com/r/johnktims/folding-at-home to run the [folding@home](https://foldingathome.org) client (for doing COVID-19 protein folds) on a SAP CP cf account.  

As https://foldingathome.org/2020/03/15/coronavirus-what-were-doing-and-how-you-can-help-in-simple-terms/ says:

> We’re simulating the dynamics of COVID-19 proteins to hunt for new therapeutic opportunities.

## requirements

- SCP cf (trial) account w/ 3 GB mem to spare
- install the [SAP cloud mta build tool](https://github.com/SAP/cloud-mta-build-tool):  
  `npm install -g mbt`

## how-to

### adjust the mtad.yaml

- replace `fath-cf` with a unique identifier of your choice, e.g. `fath-cf-007`
- (optional) replace the `--user` and `--team` switches of the `FAHClient` start command with your folding-user and -team

### build the application

```bash
$> mbt assemble
$> cf deploy mta_archives fath-cf-<your_id>_0.0.6.mtar
```

### deploy

```bash
$> cf login
$> cf stop fath-cf-<your_id> # optional, when already running: stop the current app
$> cf deploy mta_archives/fath-cf-<your_id>_0.0.6.mtar
```

Watch the logs via `cf logs fath-cf-<your_id>` and hopefully, you'll see somehting like this soon:

```bash
Project: 14328 (Run 0, Clone 5036, Gen 5)
Unit: 0x000000069bf7a4d65e6c6b02ee3ce7e1
Reading tar file core.xml
Reading tar file frame5.tpr
Digital signatures verified
Calling: mdrun -s frame5.tpr -o frame5.tr
Steps: first=1250000 total=250000
Completed 1 out of 250000 steps (0%)
...
```

## references

https://foldingforum.org/viewtopic.php?f=24&t=32463 says:

  If you're running a project that's listed here, you're fighting COVID-19:  
  CPU : 14328 - 14329 - 14530 - 14531  
  GPU : 11741 to 11764
