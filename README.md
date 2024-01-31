# Optimizing AWS Client VPN Concurrent User Management

The repository introduces a solution along with a AWS Cloudformation template for easy deployment which uses a Lambda handler that incorporates custom logic to manage concurrent user logins effectively. By implementing this handler, users will gain insights into authorizing only the latest connection upon user authentication, automatically terminating older connections and thereby enhancing security while maintaining a streamlined user experience in their AWS Client VPN environment.

## Use Case
A potential use case in real world could be let's say an organization relies on AWS Client VPN for secure remote access to its resources. In this environment, users occasionally log in from different devices simultaneously using the same credentials, creating a potential security vulnerability. With the default behavior allowing all these connections to establish, there is a risk of unauthorized access and compromised access controls. To address this challenge, the introduced Lambda handler provides a practical solution by implementing custom logic. This use case demonstrates how organizations can enhance security and streamline user access in their AWS Client VPN environment, ensuring that only the latest connection is authorized while automatically terminating older connections associated with identical user credentials.
## Architecture

![Architecture diagram] (./architecture.png)

## Assumptions

1. When setting up an AWS Client VPN, you have the option to configure mutual authentication, which requires clients to present a client certificate to authenticate themselves to the VPN endpoint. The proposed solution request a new public certificate which requires a fully qualified domain name as an input parameter.
2. AWS Client VPN Endpoint supports several authentication options for users connecting to the VPN endpoint as mentioned in this [documentation](https://docs.aws.amazon.com/vpn/latest/clientvpn-admin/client-authentication.html). This solution uses AWS Managed Microsoft AD for client authentication.

## Deployment

To deploy this solution you can use either AWS Console or AWS CLI to deploy this solution in your AWS Account. 
### Using AWS Console
You can follow the steps mentioned in this [documentation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html) to create a AWS Cloudformation stack.
### Using AWS CLI
>*Make sure you configure the AWS CLI in your terminal. You can follow the steps mentioned in this [documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html).*
1. Clone the repository in your local machine.
2. Use the following command to deploy this solution from the repository directory. Replace the parameters with their respective values
	 >  `aws cloudformation deploy --template-file ClientVPN-cfn.yaml --stack-name <Stack_Name> --capabilities CAPABILITY_NAMED_IAM --parameter-overrides MicrosoftADPW=<your_AD_Password> MicrosoftADShortName=<MicrosoftADShortName> subnetID1=<subnetID1> subnetID2=<subnetID1> vpcID=<vpcID> ClientCidrBlock=<ClientCidrBlock> DomainName=<DomainName>`
 
  * **MicrosoftADPW**: AWS Managed Microsoft AD password
  * **MicrosoftADShortName**: The NetBIOS name for your domain, such as CORP
  * **subnetID1**: The ID of the subnet in which to create the client VPN endpoint
  * **subnetID2**: The ID of the subnet in which to create the client VPN endpoint
  * **vpcID**: The ID of the VPC in which to create the client VPN endpoint
  * **ClientCidrBlock**: The CIDR block to associate with the client VPN endpoint
  * **DomainName**: The fully qualified domain name for the certificate
	
