# 💾 0x4447 S3 Private Drive

This stack was created just speed up the process of creating a S3 bucket as a network storage with versioning configured, and a 30 day window to recover the deleted or older versions of the file. Since this is something we do over and over for our clients, we decided to describe the configuration once, and just within minutes be on our way.

The stack will also create a special IAM Group with a in-line policy that gives any user that is attach to this group the correct rights to interact with the S3 objects. This policy takes in account the enabled versioning, to make it all work.

# DISCLAIMER!

This stack is available to anyone at no cost, but on an as-is basis. 0x4447 LLC is not responsible for damages or costs of any kind that may occur when you use the stack. You take full responsibility when you use it.

# How to deploy

<a target="_blank" href="https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=zer0x4447-S3-Drive-Private&templateURL=https://s3.amazonaws.com/0x4447-drive-cloudformation/s3-drive-private.json">
<img align="left" style="float: left; margin: 0 10px 0 0;" src="https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png"></a>

All you need to do to deploy this stack is click the button to the left and follow the instructions that CloudFormation provides in your AWS Dashboard. Alternatively you can download the CF file from [here](https://s3.amazonaws.com/0x4447-drive-cloudformation/s3-drive-private.json).

# What will deploy?

The stack takes advantage of AWS S3 and AWS IAM Groups. You'll get:

- 1x S3 Bucket
- 1x IAM Group

# Manual work

After the stack is deployed the only thing left is to create a IAM user or use a pre-existing one and attach to this user the IAM Group that was created with the bare minimum actions needed to work with the bucket.

# How to recover deleted files in S3

When you have S3 versioning enabled there is no UI in the AWS Dashboard that can help you recover all the files at once – you can only recover individual files. To recover everything that was delete the command line bellow is going to recover those files for you.

```
AWS_ACCESS_KEY_ID=KEY \
AWS_SECRET_ACCESS_KEY=SECRET \
aws s3api list-object-versions --bucket BUCKET_NAME --output text | \
grep -E "^DELETEMARKERS" | \
awk '{FS = "[\t]+"; print "aws s3api delete-object --bucket BUCKET_NAME --key \42"$3"\42 --version-id "$5";"}' >> undelete_script.sh
```

Once the CLI finishes working, you'll end up with the `undelete_script.sh` file, which will contain in each line a separated action to remove the `delete` flag from the S3 object. Make sure to review this file, and then set the it to be executable `chmod +x undelete_script.sh` and run it.

# How to work with this project

When you want to deploy the stack, the only file you should be interested in is the `CloudFormation.json` file. If you'd like to modify the stack, we recommend that you use the [Grapes framework](https://github.com/0x4447/0x4447-cli-node-grapes), which was designed to make it easier to work with the CloudFormation file. If you'd like to keep your sanity, never edit the main CF file 🤪.

# The End

If you enjoyed this project, please consider giving it a 🌟. And check out our [0x4447 GitHub account](https://github.com/0x4447), where you'll find additional resources you might find useful or interesting.

## Sponsor 🎊

This project is brought to you by 0x4447 LLC, a software company specializing in building custom solutions on top of AWS. Follow this link to learn more: https://0x4447.com. Alternatively, send an email to [hello@0x4447.email](mailto:hello@0x4447.email?Subject=Hello%20From%20Repo&Body=Hi%2C%0A%0AMy%20name%20is%20NAME%2C%20and%20I%27d%20like%20to%20get%20in%20touch%20with%20someone%20at%200x4447.%0A%0AI%27d%20like%20to%20discuss%20the%20following%20topics%3A%0A%0A-%20LIST_OF_TOPICS_TO_DISCUSS%0A%0ASome%20useful%20information%3A%0A%0A-%20My%20full%20name%20is%3A%20FIRST_NAME%20LAST_NAME%0A-%20My%20time%20zone%20is%3A%20TIME_ZONE%0A-%20My%20working%20hours%20are%20from%3A%20TIME%20till%20TIME%0A-%20My%20company%20name%20is%3A%20COMPANY%20NAME%0A-%20My%20company%20website%20is%3A%20https%3A%2F%2F%0A%0ABest%20regards.).
