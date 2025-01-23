# helm_charts

the matching "git page" is at https://cloudhiro.github.io/helm-charts-chi

to install this run:  
```bash 
helm install -n chi-metrics-go --create-namespace chi-metrics-go  \
--set collectorDeployment.initialToken=<initial token> \
cloudhiro/chi-metrics

```

To get the initial token please visit https://ops.cloudhiro.com/AWS/AWSShowExternalID.php or contact your account manager.