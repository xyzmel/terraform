# Azure VM Setup with Terraform

This Terraform script helps you provision multiple Linux virtual machines (VMs) in Microsoft Azure, all within the same subnet. It also configures a Network Security Group (NSG) to allow SSH access and dynamically assigns private IPs to the VMs.

## Prerequisites

Before using this script, ensure you have the following tools installed and configured:

1. **Terraform**: Download and install Terraform from [terraform.io](https://www.terraform.io/downloads.html).
2. **Azure CLI**: Install Azure CLI from [azure.microsoft.com](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli).
3. **Azure Subscription**: You need an active Azure subscription.

## Setup Instructions

### 1. Clone the Repository

Clone the repository to your local machine:

```bash
git clone https://github.com/xyzmel/terraform.git
cd terraform
```

### 2. Initialize Terraform

Navigate to the folder containing the Terraform script, and initialize the Terraform environment:

```bash
terraform init
```

This command installs the required Terraform providers and prepares the working directory.

### 3. Configure `main.tf` Variables

In the `main.tf` file, update the default values of the variables defined under the `# Variables` section:

- `resource_group_name`: The name of the Azure resource group (default: `myResourceGroup`).
- `location`: The Azure region where resources will be created (default: `East US`).
- `subnet_cidr`: The subnet CIDR block for the virtual network (default: `10.0.1.0/24`).
- `vm_count`: The number of VMs to create (default: `3`).
- `vm_size`: The size of the VM (default: `Standard_B2s`).
- `admin_username`: The administrator username for the VMs (default: `azureuser`).
- `admin_password`: The administrator password for the VMs (default: `P@ssw0rd123!`).

### 4. Plan the Deployment

Run the following command to preview the resources that will be created:

```bash
terraform plan
```

This will give you a summary of the resources that will be provisioned, allowing you to review the configuration before applying it.

### 5. Apply the Configuration

To create the resources in Azure, run:

```bash
terraform apply
```

You will be prompted to confirm the action by typing `yes`. Terraform will then create the resource group, virtual network, subnet, network interfaces, and VMs.

### 6. Access the VMs

Once the deployment is complete, Terraform will output the private and public IP addresses of the created VMs.

- **Private IPs**: Used for internal communication within the virtual network.
- **Public IPs**: Used for SSH access from outside the virtual network.

To access a VM, use the following command (replace `vm-public-ip` with the actual public IP address):

```bash
ssh azureuser@vm-public-ip
```

### 7. Clean Up Resources

When you're done, you can delete all resources created by Terraform by running:

```bash
terraform destroy
```

This will remove the resource group, virtual network, subnet, and all VMs.

## Troubleshooting

- Ensure that you have the necessary permissions in Azure to create resources like VMs, networks, and NSGs.
- Double-check the values in the `main.tf` file if you encounter issues with the script or Azure provisioning.

## Notes

- The script uses an Ubuntu 20.04 LTS image by default for the virtual machines. You can change the image in the `source_image_reference` block to another Linux distribution if needed.
- The default security rule allows SSH access (port 22). Modify or add additional security rules as required.
- The password for the admin user is defined as `P@ssw0rd123!` for demonstration purposes. **It's highly recommended to use a more secure password or integrate Azure Key Vault for sensitive data**.
