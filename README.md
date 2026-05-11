# Use Amazon EC2 for cost-efficient cloud gaming with pay-as-you-go pricing

This repository contains the full source code that is used in the blog post [Use Amazon EC2 for cost-efficient cloud gaming with pay-as-you-go pricing](https://aws.amazon.com/blogs/compute/use-amazon-ec2-for-cost-efficient-cloud-gaming-with-pay-as-you-go-pricing/).

## Solution Overview
<p align="center">
  <img src="img/GraphicsOnG_architecture.png" />
</p>

### Prerequisites

- An [AWS account](https://signin.aws.amazon.com/signin?redirect_uri=https%3A%2F%2Fportal.aws.amazon.com%2Fbilling%2Fsignup%2Fresume&client_id=signup)
- Installed and authenticated [AWS CLI](https://docs.aws.amazon.com/en_pv/cli/latest/userguide/cli-chap-install.html) (authenticate with an [IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started.html) user or an [AWS STS](https://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) Security Token)
- Installed and setup [AWS Cloud Development Kit (AWS CDK)](https://docs.aws.amazon.com/cdk/latest/guide/getting_started.html)
- Installed Node.js, TypeScript and git
- AWS EC2 KeyPair (.pem)


### Let’s get you started

#### 1. Make sure you completed the prerequisites above and cloned this repo.

```
git clone https://github.com/aws-samples/cloud-gaming-on-ec2-instances.git 
```

#### 2. Open the repository in your preferred IDE and familiarize yourself with the structure of the project.

```
.
├── cdk             CDK code that defines the environment
└── img             Images used in this README
```


#### 3. Install dependencies

node.js dependencies are declared in a `package.json`. This project contains a `package.json` file in the `cdk` folder. 

Navigate into `cdk` folder it and run `npm install` 
```
$ cd cdk 
$ npm install
```


#### 4. Configure your environment

Before you can deploy the stack, you need to review the config. Navigate to `cdk/bin/cloud-gaming-on-ec2.ts` and review / update the following parameters:

- `ACCOUNT`: (required) The account id you want to deploy the stack in
- `REGION`: (required) The region you want to deploy the stack in
- `NICE_DCV_DISPLAY_DRIVER_URL`: The download URL of the Amazon DCV Virtual Display Driver (formerly NICE DCV) for EC2. You can leave this unless the link is broken or you want to use a different version.
- `NICE_DCV_SERVER_URL`: The download URL of the Amazon DCV Server (formerly NICE DCV). You can leave this unless the link is broken or you want to use a different version.
- `InstanceSize`: Sets the size of the EC2 Instance. Defaults to `g6.xlarge`, `g6e.xlarge`, `gr6.xlarge` ,`g5.xlarge`, `g4dn.xlarge`, and `g4ad.xlarge` respectively. 
- `associateElasticIp`: Controls if an Elastic IP address will be created and added to the EC2 instance.
- `EC2_KEYPAIR_NAME`: (required) The name of the EC2 key pair you will use to connect to the instance. Make sure to have access to the respective .pem file.
- `VOLUME_SIZE_GIB`: The size of the root EBS volume. Around 20 GB will be used for the Windows installation, the rest will be available for your software. Note: Some EC2 Instance Types include instance store which can be initalized. 
- `OPEN_PORTS`: Access from these ports will be allowed. Per default this will only allow access for Amazon DCV (formerly NICE DCV) on port 8443
- `ALLOW_INBOUND_CIDR`: Access from this CIDR range will be allowed. Per default this will allow access from /0, but I recommend to restrict this to your IP address only.
- `GRID_SW_CERT_URL`: (Only for g4dn/g5 instances) The NVIDIA driver requires a certificate file which can be downloaded from Amazon S3. You can leave this unless the link is broken or you want to use a different certificate.
- `tags`: A list of resource tags that will be added to every taggable resource in the stack.
- `SEVEN_ZIP_URL`: Update to the latest 7zip .msi version as it is required for the automated NVIDIA driver install. 
- `CHROME_URL`: Installs Google Chrome Enterprise x64.

#### 5. Deploy your application

The CDK code is written in TypeScript, an extension to JavaScript that adds static types and other useful features.

To run the CDK code, navigate to the `cdk` folder and run the following commands

```
$ cdk bootstrap
$ cdk deploy <StackName>
```

After bootstrapping the required resources for the CDK with `cdk bootstrap` you can then deploy the template with `cdk deploy <StackName>`. Bootstrapping is only require once.

`<StackName>` can be either `CloudGamingOnG6`, `CloudGamingOnG6E`, `CloudGamingOnGR6`, `CloudGamingOnG4DN`, `CloudGamingOnG4AD` or `CloudGamingOnG5`, depending on the instance type you want to use.

The following table gives an overview over the expected graphics performance at 1080p, expressed as 3DMark Time Spy (v x.xx) scores.

| Instance Type | 3DMark Score | On-demand Price (us-east-1, USD, 04/09) | Price-performance (3DMark points / $) |
|--------------|--------------|-----------------------------------------|---------------------------------------|
| g4dn.xlarge  | 4300         | $0.71                                   | 6056                                  |
| g4dn.2xlarge | 4800         | $1.12                                   | 4286                                  |
| g4dn.4xlarge | 6000         | $1.94                                   | 3093                                  |
| g4ad.xlarge  | 5100         | $0.56                                   | 9107                                  |
| g4ad.2xlarge | 6600         | $0.91                                   | 7253                                  |
| g4ad.4xlarge | 7600         | $1.60                                   | 4750                                  |
| g5.xlarge    | 6800         | $1.19                                   | 5714                                  |
| g5.2xlarge   | 10200        | $1.58                                   | 6456                                  |
| g5.4xlarge   | 13000        | $2.36                                   | 5508                                  |
| g6.xlarge    | N/A        | $0.99                                   | N/A                                  |
| g6.2xlarge   | N/A        | $1.35                                   | N/A                                  |
| g6.4xlarge   | N/A        | $2.10                                   | N/A                                  |
| g6e.xlarge   | N/A        | $2.00                                   | N/A                                  |
| g6e.2xlarge   | N/A        | $2.60                                   | N/A                                  |
| g6e.4xlarge   | N/A        | $3.70                                   | N/A                                  |
| gr6.4xlarge   | N/A        | $2.28                                   | N/A                                  |

Stack completion usually takes `8-15` minutes.

#### 6. Create your personal gaming AMI

Follow the instructions in the associated blog post [Use Amazon EC2 for cost-efficient cloud gaming with pay-as-you-go pricing](https://aws.amazon.com/blogs/compute/use-amazon-ec2-for-cost-efficient-cloud-gaming-with-pay-as-you-go-pricing/).

#### 7. Finish Sunshine + Moonlight setup (one time, after first deploy)

Sunshine is installed and running as a Windows service by cloud-init, but the first-run credentials and Moonlight pairing must be done interactively. DCV stays as the management/recovery path; Moonlight becomes the streaming path.

##### 7a. Verify Sunshine installed cleanly

Connect via the **DCV client** at `https://<public-ip>:8443` (username `Administrator`, password decrypted from `windows-machine.pem` via `aws ec2 get-password-data`). Then in PowerShell:

```powershell
Test-Path "C:\Program Files\Sunshine\sunshine.exe"
Get-Service SunshineService | Format-Table Name, Status, StartType
```

Expected: `True` and `SunshineService Running Automatic`.

##### 7b. Create Sunshine admin credentials

Inside the DCV session, open Chrome (installed by cloud-init) and go to `https://localhost:47990`. Accept the self-signed cert warning (**Advanced → Proceed to localhost**) and create the Sunshine admin username/password. These are separate from the Windows Administrator account — you'll use them only for the Sunshine Web UI.

##### 7c. Pair Moonlight

1. On your local machine, install Moonlight from https://moonlight-stream.org.
2. In Moonlight: **Add Host** → enter the instance's Elastic IP → it displays a 4-digit PIN.
3. In Sunshine's Web UI (`https://localhost:47990`): **PIN** tab → enter the PIN → paired.
4. Back in Moonlight, click the host → launch **Desktop** → you should get a working stream.

To disconnect a Moonlight session: **Ctrl + Alt + Shift + Q** (on macOS: **Ctrl + Option + Shift + Q**).

##### 7d. Recommended stream settings

In Moonlight on your local machine (gear icon → Settings):

| Setting | Recommendation |
|---|---|
| Resolution | Match your local display (1920×1080 / 2560×1440 / 3840×2160) |
| Frame rate | 60 fps |
| Bitrate | 40-80 Mbps for 1080p60, 80-120 Mbps for 1440p60, 150+ Mbps for 4K60 |
| V-Sync | On |
| HDR | Off (NVIDIA gaming drivers on EC2 don't support HDR over Sunshine) |

Inside the streamed session, also bump the Windows desktop resolution to match (right-click desktop → **Display settings** → Display resolution). Sunshine captures whatever Windows is rendering — if Windows is at 1024×768 you'll see an upscaled, blurry 1024×768 in Moonlight regardless of client settings. The DCV virtual display driver supports up to 4K.

##### 7e. Session behavior — DCV and Moonlight together

- DCV's "automatic console session" (configured by the registry tweaks in `cdk/lib/base.ts`) gives Sunshine a desktop to capture. You can **disconnect** the DCV client and Moonlight keeps working — but **don't sign out of Windows** (Start → Sign out), that destroys the session and Sunshine loses its capture target.
- Rebooting or stop+start of the instance ends the console session. After it comes back up, you'll need to connect via DCV once to re-establish the session before Moonlight will stream — unless you enable AutoAdminLogon (next step).

##### 7f. (Optional) Streaming after reboot without DCV

To make Moonlight work immediately after every reboot without DCVing in first, enable Administrator auto-login. In an elevated PowerShell inside the instance:

```powershell
$pw = '<your-Administrator-password>'
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v AutoAdminLogon /t REG_SZ /d 1 /f
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v DefaultUserName /t REG_SZ /d Administrator /f
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v DefaultPassword /t REG_SZ /d $pw /f
Restart-Computer
```

Caveat: this stores the password in plaintext in the registry, readable by anyone with admin / VHD access. For a hardened version use Sysinternals [`Autologon.exe`](https://learn.microsoft.com/en-us/sysinternals/downloads/autologon), which stores it in the LSA secret store instead.

##### 7g. Stop the instance when idle

A running g6.2xlarge is ~$1.35/hr in `us-west-2`. Stop it when you're not using it:

```
aws ec2 stop-instances --instance-ids <instance-id> --region us-west-2
aws ec2 start-instances --instance-ids <instance-id> --region us-west-2
```

The Elastic IP persists across stop/start at no extra charge while attached.

## Useful CLI commands

List EC2 key pairs

```
$ aws ec2 describe-key-pairs --query 'KeyPairs[*].KeyName' --output table
```
Create a new key pair and the PEM file to store your private key
```
KEY_NAME=GamingOnEc2
$ aws ec2 create-key-pair --key-name $KEY_NAME --query 'KeyMaterial' --output text > $KEY_NAME.pem
```
Get Password Data from your launched instance.
```
$ aws ec2 get-password-data --instance-id <INSTANCE_ID> --priv-launch-key /path/to/GamingOnEc2.pem
```
Start / Stop an EC2 instance
```
$ aws ec2 start-instances --instance-ids INSTANCE_ID
$ aws ec2 stop-instances --instance-ids INSTANCE_ID
```

Creates an Amazon Machine Image from an EC2 instance
```
$ aws ec2 create-image --instance-id <YOUR_INSTANCE_ID> --name <THE_NAME_OF_YOUR_AMI>
```

Starts a new EC2 instance from a launch template
```
$ aws ec2 run-instances --image-id <YOUR_AMI_ID> --launch-template LaunchTemplateName=<LAUNCH_TEMPLATE_NAME> --query "Instances[*].[InstanceId, PublicIpAddress]" --output table
```

List your instances
```
$ aws ec2 describe-instances --query "Reservations[*].Instances[*].[ImageId, InstanceType, VpcId, State.Name, PublicIpAddress, LaunchTime]" --output table
```
Deploy some stacks at once, without rollback and dont require approval for IAM resources
```
$ cdk deploy CloudGamingOnG6 CloudGamingOnG6E CloudGamingOnGR6 --no-rollback --concurrency=3 --require-approval=never
```
Deploy all stacks at once, without rollback and dont require approval for IAM resources
```
$ cdk deploy --all --no-rollback --concurrency=6 --require-approval=never
```
List all stacks
```
$ cdk list
```

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.
