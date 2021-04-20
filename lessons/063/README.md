# Terraform Blue Green Deployment & Canary Deployment 🔵🟢 AWS | ALB | EC2

[Step by Step Tutorial](https://youtu.be/<id>)

Terraform docs: https://github.com/hashicorp/learn-terraform-advanced-deployments

## Shift Traffic to Green Deployment
```bash
$ terraform apply -var 'traffic_distribution=blue' -var 'enable_green_env=false' -auto-approve
$ terraform apply -var 'traffic_distribution=blue-90' -var 'enable_green_env=true' -auto-approve
$ terraform apply -var 'traffic_distribution=split' -var 'enable_green_env=true' -auto-approve
$ terraform apply -var 'traffic_distribution=green-90' -var 'enable_green_env=true' -auto-approve
$ terraform apply -var 'traffic_distribution=green' -var 'enable_blue_env=false' -var 'enable_green_env=true'
```

## Scale Down Blue Environment
```bash
terraform apply -var 'traffic_distribution=green' -var 'enable_blue_env=false'
```

## Test Version
```bash
for i in `seq 1 1000`; do curl $(terraform output -raw lb_dns_name); done
```
