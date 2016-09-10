# MyAWS

MyAWS is a simple command line tool for managing my aws resources.

The aws-cli is useful but too generic. It has many arguments and options and generates huge JSON outputs. But, in most cases, my interesting resources are the same. By setting my favorite default values, myaws provides a simple command line interface.

Note that MyAWS is under development and its interface is unstable.

# Installation

```
$ go get github.com/minamijoyo/myaws
```

or

Download latest release binary, unzip and chmod.

https://github.com/minamijoyo/myaws/releases

```
$ unzip myaws_v0.0.1_darwin_amd64.zip
$ chmod +x myaws
$ ./myaws
```

# Configuration
## Required
MyAWS invokes AWS API call via aws-sdk-go.
Export environment variables for your AWS credentials:

```
$ export AWS_ACCESS_KEY_ID=XXXXXX
$ export AWS_SECRET_ACCESS_KEY=XXXXXX
$ export AWS_DEFAULT_REGION=XXXXXX
```

or set your credentials in `$HOME/.aws/credentials` :

```
[default]
aws_access_key_id = XXXXXX
aws_secret_access_key = XXXXXX
```

AWS region can be set in Environment variable ( `AWS_DEFAULT_REGION` ), configuration file ( `$HOME/.myaws.yaml` ) , or command argument ( `--region` ).

## Optional

Configuration file is optional.

MyAWS read default configuration from `$HOME/.myaws.yml`

A sample configuration looks like the following:

```
profile: default
region: ap-northeast-1
ec2:
  ls:
    all: false
    fields:
      - InstanceId
      - InstanceType
      - PublicIpAddress
      - PrivateIpAddress
      - StateName
      - LaunchTime
      - Tag:Name
      - Tag:attached_asg

# Usage

```
$ myaws --help
MyAWS is a simple command line tool for managing my aws resources

Usage:
  myaws [command]

Available Commands:
  ec2         Manage EC2 resources
  version     Print version

Flags:
      --config string    config file (default $HOME/.myaws.yml)
      --profile string   AWS profile (default none and used AWS_ACCESS_KEY_ID/AWS_SECRET_ACCESS_KEY environment variables.)
      --region string    AWS region (default none and used AWS_DEFAULT_REGION environment variable.

Use "myaws [command] --help" for more information about a command.
```

```
$ myaws ec2 --help
Manage EC2 resources

Usage:
  myaws ec2 [flags]
  myaws ec2 [command]

Available Commands:
  ls          List EC2 instances
  start       Start EC2 instances
  stop        Stop EC2 instances

Global Flags:
      --config string    config file (default $HOME/.myaws.yml)
      --profile string   AWS profile (default none and used AWS_ACCESS_KEY_ID/AWS_SECRET_ACCESS_KEY environment variables.)
      --region string    AWS region (default none and used AWS_DEFAULT_REGION environment variable.

Use "myaws ec2 [command] --help" for more information about a command.
```

```
$ myaws ec2 ls --help
List EC2 instances

Usage:
  myaws ec2 ls [flags]

Flags:
  -a, --all                  List all instances (by default, list running instances only)
  -t, --filter-tag string    Filter instances by tag, such as "Name:app-production". The value of tag is assumed to be a partial match (default "Name:")
  -T, --output-tags string   Output tags list separated by commas, such as "Name,attached_asg" (default "Name")

Global Flags:
      --config string    config file (default $HOME/.myaws.yml)
      --profile string   AWS profile (default none and used AWS_ACCESS_KEY_ID/AWS_SECRET_ACCESS_KEY environment variables.)
      --region string    AWS region (default none and used AWS_DEFAULT_REGION environment variable.
```

# Example

```
$ myaws ec2 ls
i-0f48fxxxxxxxxxxxx     t2.micro        52.197.xxx.xxx  10.193.xxx.xxx    running 1 minute ago    proxy
i-0e267xxxxxxxxxxxx     t2.medium       52.198.xxx.xxx  10.193.xxx.xxx    running 2 days ago      app
i-0fdaaxxxxxxxxxxxx     t2.large        52.197.xxx.xxx  10.193.xxx.xxx    running 1 month ago     batch
```

# LICENCE

MIT

