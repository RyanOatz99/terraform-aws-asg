# terraform-aws-openshift 

Autoscaling Group module for AWS

## Features
* Default security group for each ASG with possibility to set management network access
* Tagging Name, Environment
* Multi AZ
* ELB mapping


## Usage

```hcl
module "app" {
  source                      = "github.com/odzhu/terraform-aws-asg"
  subnet_ids                  = "${var.private_subnet_ids}"
  environment                 = "${var.environment}"
  name                        = "${var.app_name}"
  vpc_id                      = "${var.vpc_id}"
  instance_type               = "${var.app_instance_type}"
  instance_profile            = "${aws_iam_instance_profile.app.name}"
  ami                         = "${var.app_ami}"
  admin_ssh_key               = "${aws_key_pair.admin_key.key_name}"
  user_data                   = "${data.template_file.app.rendered}"
  load_balancers              = ["${aws_elb.infra.name}"]
  management_net              = "${var.management_net}"
  associate_public_ip_address = "false"
  min_size                    = "${var.app_node_count}"
  max_size                    = "${var.app_node_count + 2}"
}
```