#####################################################################

#         POWERSHELL ALB/AGW/TM Exercise                             #

#####################################################################
# 
# Welcome to this Powershell Lab
# We will build the following scenario in this lab:
#
# You will create 2 virtual hosting solutions on the East and West coasts with 
# a solution to manage traffic between the two, and failover. 
#   Traffic Manager --> East & West DR 
#   EAST --> Resource Group - AGW + VNETs/Subnets - 2 VM's in Availability Set
#   WEST --> Resource Group - ALB + VNETs/Subnets - 2 VM's in Availability Set
# Before we begin:
# We need to make sure subscription has services registered that we want...
# Navigate to: 
# - portal / subscriptions / { select } / resource providers 
# Register the following: Compute, Network, Storage, DocumentDB, Web
# Note: If you have other labs you want to try, you may need to add those
# resource providers using this same process.
#
# The following examples are using Powershell
# Documentation for Powershell is best found here:
# https://docs.microsoft.com/en-us/powershell/azure/overview?view=azurermps-5.2.0
# see bottom menu item: reference 
#

# Login to Az Account - this will open browser in popup and you need to authenticate in the portal
Login-AzureRmAccount

# Clear the screen
cls


####################################################

#         Application Gateway Scenario
#
#   EAST --> Resource Group - AGW + VNETs/Subnets - 2 VM's in Availability Set

####################################################

#   Resource Group

# Create Resource group
# Name is not unigue in Azure
# Important: Resource Group NAME IS used through the rest of this script, 
# so if you change it, you will have to change it elsewhere
# More info on Resource Groups
# https://docs.microsoft.com/en-us/azure/azure-resource-manager/management-groups-overview
New-AzureRMResourceGroup -name AGWPS -location eastus

###################################################
# 
# Storage accounts - 

# create Standard Storage account for boot diagnostics (select a new name, name must be unique in AZ)
# name is lowercase alpha and numbers - no special characters
# note: the name is not used in the rest of this script so you don't have to be too picky
New-AzureRmStorageAccount –StorageAccountName ************ -Location eastus -ResourceGroupName AGWPS -SkuName Standard_LRS

# Premium Storage No longer necessary for managed disks virtual machines 
# Create a new premium storage account for our vm's (select a new name, must be unique in AZ)
# New-AzureRmStorageAccount –StorageAccountName ************ -Location eastus -ResourceGroupName AGWPS -SkuName Premium_LRS

##############################################################################

#           Network Security Group (NSG's)

##############################################################################
# Create Network Security Group (NSG's)
# About NSG's
# https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg

# To Create Network Security Group (NSG's)
# In Powershell, this is different than CLI. Here we create the rules, load in variables, 
#   and then execute command to create and set rules
#
# NSG rules - define traffic in/out, allow/deny
$rule1 = New-AzureRmNetworkSecurityRuleConfig -Name web-rule -Description "Allow HTTP" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
-SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 80

$rule2 = New-AzureRmNetworkSecurityRuleConfig -Name rdp-rule -Description "Allow RDP" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 100 `
-SourceAddressPrefix Internet -SourcePortRange * `
-DestinationAddressPrefix * -DestinationPortRange 3389

# Load all values into a variable that will execute the new-NSG powershell
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName agwps -Location eastus `
-Name "agwps-nsg" -SecurityRules $rule1,$rule2

# This runs the script for NSG
$nsg
 
##############################################################################
#
#       IP Addresses for Resources 
#
# Create public ip addresses for resources, 2 vm's 
# change the dns-name to be unique. lowercase numeric only, no special characters
# 
# Note: we are using static for this lab, just so we have a consistent IP address to go to for testing.
# More common use case is to use dynamic IP addresses for VM's.

New-AzureRmPublicIpAddress -Name agwpsvm-ip01 -ResourceGroupName agwps  `
-AllocationMethod Static -DomainNameLabel *************** -Location eastus

New-AzureRmPublicIpAddress -Name agwpsvm-ip02 -ResourceGroupName agwps  `
-AllocationMethod Static -DomainNameLabel *************** -Location eastus

# About Azure Public IP addresses
# https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-ip-addresses-overview-arm


# Get Public IP Address
Get-AzureRmPublicIpAddress -ResourceGroupName agwps | select name, ipaddress 

# Remove-AzureRmPublicIpAddress -name <name> -ResourceGroupName <g>


##############################################################################
#
#       Virtual Network  (VNet) 
#
#  Create Virtual network with Subnets for AGW and VM's
#  Note: App Gateway needs to be in a seperate Subnet from VMs.
#  About Azure VNet's
#  https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview

# 
$Subnet = New-AzureRmVirtualNetworkSubnetConfig -Name "AGWPSSubnet" -AddressPrefix 10.0.0.0/24

$VNet = New-AzureRmvirtualNetwork -Name "AGWPSVnet" -ResourceGroupName "AGWPS" `
-Location "East US" -AddressPrefix 10.0.0.0/16 -Subnet $Subnet 

$VNet = Get-AzureRmvirtualNetwork -Name "AGWPSVnet" -ResourceGroupName "AGWPS"

# create 2nd subnet in Vnet for VM's 

$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName AGWPS -Name AGWPSVnet

#add a subnet to the new vnet variable
Add-AzureRmVirtualNetworkSubnetConfig -Name AGWVMs `
-VirtualNetwork $vnet -AddressPrefix 10.0.1.0/24

Set-AzureRmVirtualNetwork -VirtualNetwork $vnet


##############################################################################
#
#   Virtual Machines and Availability Set
#
##############################################################################
# About Azure Windows VM's 
# https://docs.microsoft.com/en-us/azure/virtual-machines/windows/overview
# We are creating Windows VM's and setting them up as Webservers for our Lab

# create new availability set for VM's defining default + update domains
### We are going to create 2 vm's in vnet / subnet / availability set

New-AzureRmAvailabilitySet -ResourceGroupName "AGWPS" -Name "AGWPS-ASet" -Location eastus `
-PlatformFaultDomainCount 2 -PlatformUpdateDomainCount 2 -sku Aligned

# About Availability Sets
# https://docs.microsoft.com/en-us/azure/virtual-machines/windows/tutorial-availability-sets?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#availability-set-overview



#######################################################################
#
# Create the VM #1 and nic - assign IP created above 
# Note: Change username and password for the VM you are about to create with this next line
# What types of VM's are these? We are using "managed disks". 
# More about managing VM's with Powershell here
# https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-powershell

#Create the VM1 with...
$AvailabilitySet = Get-AzureRmAvailabilitySet -ResourceGroupName AGWPS -name "AGWPS-ASet"
$vnet = Get-AzureRmVirtualNetwork -Name AGWPSVnet -ResourceGroupName AGWPS 
$nsg = Get-AzureRmNetworkSecurityGroup -name agwps-nsg -ResourceGroupName AGWPS
$pip = Get-AzureRmPublicIpAddress -Name agwpsvm-ip01 -ResourceGroupName AGWPS 
# use subnet[1] because [0] has application gateway
$nic = New-AzureRmNetworkInterface -Name agwpsvmNIC-01 -ResourceGroupName AGWPS -Location eastus `
    -SubnetId $vnet.Subnets[1].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
$cred = Get-Credential
# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName AGWPSvm-01 -VMSize Standard_DS1_v2 -AvailabilitySetID $AvailabilitySet.Id | `
    Set-AzureRmVMOperatingSystem -Windows -ComputerName AGWPSvm-01 -Credential $cred | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id 
    
# Create the virtual machine with New-AzureRmVM.
New-AzureRmVM -ResourceGroupName AGWPS -Location eastus -VM $vmConfig



############
##VM2 - repeat of steps above
$AvailabilitySet = Get-AzureRmAvailabilitySet -ResourceGroupName AGWPS -name "AGWPS-ASet"
$vnet = Get-AzureRmVirtualNetwork -Name AGWPSVnet -ResourceGroupName AGWPS 
$nsg = Get-AzureRmNetworkSecurityGroup -name agwps-nsg -ResourceGroupName AGWPS
$pip = Get-AzureRmPublicIpAddress -Name agwpsvm-ip02 -ResourceGroupName AGWPS 
# use subnet[1] because [0] has application gateway
$nic = New-AzureRmNetworkInterface -Name agwpsvmNIC-02 -ResourceGroupName AGWPS -Location eastus `
    -SubnetId $vnet.Subnets[1].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
$cred = Get-Credential
# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName AGWPSvm-02 -VMSize Standard_DS1_v2 -AvailabilitySetID $AvailabilitySet.Id | `
    Set-AzureRmVMOperatingSystem -Windows -ComputerName AGWPSvm-02 -Credential $cred | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id 
    
# Create the virtual machine with New-AzureRmVM.
New-AzureRmVM -ResourceGroupName AGWPS -Location eastus -VM $vmConfig

# Next step: RDP to Machines and turn them into simple Web Servers 
# Edit default web page so we can identify machines in tests

$pip1 = Get-AzureRmPublicIpAddress -Name agwpsvm-ip01 -ResourceGroupName AGWPS 
$pip2 = Get-AzureRmPublicIpAddress -Name agwpsvm-ip02 -ResourceGroupName AGWPS

# install webserver tools - run powershell on windows vm - copy & run on VMS
# Install-WindowsFeature -name Web-Server -IncludeManagementTools

# Also, edit VM's webserver homepage with name of each machine so we can identify in tests
# c:\inetpub\wwwroot\iistart.html
#  ie.  <h1>AGW-PS-VM-01</h1> vs <h1>AGW-PS-VM-02</h1>

## RDP connection
mstsc /v: $pip1.IpAddress
mstsc /v: $pip2.IpAddress

##############################################################################

#  Create Application Gateway 

##############################################################################
# About Application Gateways
# https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-introduction

# NOTE: This example won't work until the VM's have the Web Server Tools installed in previous step.
#
# First we create an IP addy for AppGW,
# 
New-AzureRmPublicIpAddress -Name AGWpsPubip -ResourceGroupName agwps  `
-AllocationMethod Dynamic -Location eastus

# The following command: Creates the App Gateway, 
# Sets the backend pools to ip addresses in subnet with VM's

$vnet = Get-AzureRmVirtualNetwork -Name AGWPSVnet -ResourceGroupName AGWPS 
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name AGWPSSubnet -VirtualNetwork $VNet

$GatewayIPconfig = New-AzureRmApplicationGatewayIPConfiguration -Name "GatewayIp01" -Subnet $Subnet 
$Pool = New-AzureRmApplicationGatewayBackendAddressPool -Name "Pool01" -BackendIPAddresses 10.10.10.1, 10.10.10.2, 10.10.10.3
$PoolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "PoolSetting01"  -Port 80 -Protocol "Http" -CookieBasedAffinity "Disabled"
$FrontEndPort = New-AzureRmApplicationGatewayFrontendPort -Name "FrontEndPort01"  -Port 80

# get a public IP address - must be dynamic - already created above
$PublicIp = get-AzureRmPublicIpAddress -ResourceGroupName "AGWPS" -Name AGWpsPubip
$FrontEndIpConfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name "FrontEndConfig01" -PublicIPAddress $PublicIp
$Listener = New-AzureRmApplicationGatewayHttpListener -Name agwlisten -Protocol "Http" -FrontendIpConfiguration $FrontEndIpConfig -FrontendPort $FrontEndPort
$Rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name "Rule01" -RuleType basic -BackendHttpSettings $PoolSetting -HttpListener $Listener -BackendAddressPool $Pool
$Sku = New-AzureRmApplicationGatewaySku -Name "Standard_Small" -Tier Standard -Capacity 2
$Gateway = New-AzureRmApplicationGateway -Name "AppGatewayPS" -ResourceGroupName "AGWPS" `
-Location "East US" -BackendAddressPools $Pool -BackendHttpSettingsCollection $PoolSetting `
-FrontendIpConfigurations $FrontEndIpConfig  -GatewayIpConfigurations $GatewayIpConfig `
-FrontendPorts $FrontEndPort -HttpListeners $Listener -RequestRoutingRules $Rule -Sku $Sku

# note: long running process
Get-AzureRmApplicationGateway | select name, location, ResourceGroupName

### next get IP addresses for backend pools (if using ip addresses)
Get-AzureRmPublicIpAddress | select name, ipaddress

########################################################################
# Setting backend pools
# this method works because my vms have static ip addresses
# we load each IP into a variable and set that value

$pip1 = Get-AzureRmPublicIpAddress -Name agwpsvm-ip01 -ResourceGroupName agwps
$pip2 = Get-AzureRmPublicIpAddress -Name agwpsvm-ip02 -ResourceGroupName agwps
$AppGw = Get-AzureRmApplicationGateway -Name "AppGatewayPS" -ResourceGroupName agwps
$Pool = Set-AzureRmApplicationGatewayBackendAddressPool `
-ApplicationGateway $AppGw -Name "Pool01" -BackendIPAddresses $pip1.IpAddress, $pip2.IpAddress
## note: can use static values for IPs - ie. -BackendIPAddresses "52.168.14.48", "13.82.181.89"

Set-AzureRmApplicationGateway -ApplicationGateway $AppGw 

$agwip = Get-AzureRmPublicIpAddress -Name AGWpsPubip -ResourceGroupName agwps
$agwip = $agwip.IpAddress 
start-process -filepath http://$agwip

start chrome http://$agwip

# refresh until you see different machines on the backend
# start and stop different VM's to demonstrate 

#App GW VM's - stand alone in subnet in same VNet as AppGW 
stop-azurermvm -ResourceGroupName agwps -name agwpsvm-01 -force
stop-azurermvm -ResourceGroupName agwps -name agwpsvm-02 -force

#App GW VM's - stand alone in subnet in same VNet as AppGW  
start-azurermvm -ResourceGroupName agwps -name agwpsvm-01  
start-azurermvm -ResourceGroupName agwps -name agwpsvm-02 


# This completes the app gateway scenario


##############################################################################
 
#                 Azure Load Balancer Scenario                        

##############################################################################
# This Scenario builds the following
#   WEST --> Resource Group - ALB + VNETs/Subnets - 2 VM's in Availability Set

# Docs about ALB and Powershell
# https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-get-started-internet-arm-ps

cls 
##############################################################################
# Start with a New Resource Group
# Create a group for ALB

New-AzureRMResourceGroup -name ALBPS -location westus2

##############################################################################

# create standard storage account for boot diagnostics 
New-AzureRmStorageAccount –StorageAccountName **************** -Location westus2 `
-ResourceGroupName ALBPS -SkuName Standard_LRS

# Premium Storage no longer necessary with Managed Disks
# Create a new premium storage account.
# New-AzureRmStorageAccount –StorageAccountName **************** -Location westus2 `
# -ResourceGroupName ALBPS -SkuName Premium_LRS

##############################################################################
#
# Network Security Group (NSG's)
#
# First Create Network Security Group 
# Then Create NSG Rules for traffic allow/deny

# NSG rules
$rule1 = New-AzureRmNetworkSecurityRuleConfig -Name web-rule -Description "Allow HTTP" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
-SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 80

$rule2 = New-AzureRmNetworkSecurityRuleConfig -Name rdp-rule -Description "Allow RDP" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 100 `
-SourceAddressPrefix Internet -SourcePortRange * `
-DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName albps -Location westus2 `
-Name "albps-nsg" -SecurityRules $rule1,$rule2

$nsg


##############################################################################
# 
#                Virtual Networks and Subnets 
#
# We will create a Vnet + 1 Subnet:
# 1 Subnet for the VM's
# Note: ALB, unlike AGW - does not need to be in a Subnet 
#
# create new virtual network 
New-AzureRmVirtualNetwork -ResourceGroupName ALBPS -Name ALBPS-VNet `
-AddressPrefix 10.0.0.0/16 -Location westus2

# Store the virtual network object in a variable:
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName ALBPS -Name ALBPS-VNet

#add a subnet to the new vnet variable
Add-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd01 `
-VirtualNetwork $vnet -AddressPrefix 10.0.1.0/24

#Add-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd02 `
#-VirtualNetwork $vnet -AddressPrefix 10.0.2.0/24

# add subnet for backend 
# Add-AzureRmVirtualNetworkSubnetConfig -Name BackEnd01 `
# -VirtualNetwork $vnet -AddressPrefix 10.0.3.0/24

# Add-AzureRmVirtualNetworkSubnetConfig -Name BackEnd02 `
# -VirtualNetwork $vnet -AddressPrefix 10.0.4.0/24

Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

# Remove-AzureRmVirtualNetwork -name <name> -ResourceGroupName <group>

##################################################

Get-AzureRmResource | select name, resourcetype, resourcegroupname, location

##########################################################################
# Virtual IP Addresses 
#
# Create 2 New IP Addresses for 2 new VM's (DNS names must be unique)
# Change DomainNamelabel 
New-AzureRmPublicIpAddress -Name albpsvm-ip01 -ResourceGroupName albps  `
-AllocationMethod Static -DomainNameLabel **************** -Location westus2

New-AzureRmPublicIpAddress -Name albpsvm-ip02 -ResourceGroupName albps  `
-AllocationMethod Static -DomainNameLabel **************** -Location westus2

# Get Public IP Address
Get-AzureRmPublicIpAddress -ResourceGroupName albps | select name, ipaddress 
Get-AzureRMResource | Where-Object {$_.name -like "*albps*"} | select name, ipaddress


###############################################################################

# Create Availability Set's and Virtual Machines

# create availability set for 2 vm's
New-AzureRmAvailabilitySet -ResourceGroupName "albps" -Name "ALBps-ASet" -Location westus2 `
-PlatformFaultDomainCount 2 -PlatformUpdateDomainCount 2 -Sku Aligned

# VM1 - creating nic then vm 
$AvailabilitySet = Get-AzureRmAvailabilitySet -ResourceGroupName albps -name ALBps-ASet 
$vnet = Get-AzureRmVirtualNetwork -Name ALBPS-VNet -ResourceGroupName albps 
$nsg = Get-AzureRmNetworkSecurityGroup -name ALBps-nsg -ResourceGroupName albps
$pip = Get-AzureRmPublicIpAddress -Name albpsvm-ip01 -ResourceGroupName albps 
$nic = New-AzureRmNetworkInterface -Name ALBpsvmNIC-01 -ResourceGroupName albps -Location westus2 `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
$cred = Get-Credential
$vmConfig = New-AzureRmVMConfig -VMName albpsvm-01 -VMSize Standard_F2S_v2 -AvailabilitySetID $AvailabilitySet.Id | `
    Set-AzureRmVMOperatingSystem -Windows -ComputerName albpsvm-01 -Credential $cred | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id 
# Create the virtual machine with New-AzureRmVM.
# Note: Long running process 
New-AzureRmVM -ResourceGroupName albps -Location westus2 -VM $vmConfig

##########################################################################
# VM 2 - repeat of above 

$AvailabilitySet = Get-AzureRmAvailabilitySet -ResourceGroupName albps -name ALBps-ASet 
$vnet = Get-AzureRmVirtualNetwork -Name ALBPS-VNet -ResourceGroupName albps 
$nsg = Get-AzureRmNetworkSecurityGroup -name ALBps-nsg -ResourceGroupName albps
$pip = Get-AzureRmPublicIpAddress -Name albpsvm-ip02 -ResourceGroupName albps 
$nic = New-AzureRmNetworkInterface -Name ALBpsvmNIC-02 -ResourceGroupName albps -Location westus2 `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
$cred = Get-Credential
$vmConfig = New-AzureRmVMConfig -VMName albpsvm-02 -VMSize Standard_F2S_v2 -AvailabilitySetID $AvailabilitySet.Id | `
    Set-AzureRmVMOperatingSystem -Windows -ComputerName albpsvm-02 -Credential $cred | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id 
# Create the virtual machine with New-AzureRmVM.
New-AzureRmVM -ResourceGroupName albps -Location westus2 -VM $vmConfig

###########################################################################

## RDP to machines and setup inet tools and edit home page to say which vm it is
# install webserver tools - run powershell on windows vm - copy & run on VMS
# Install-WindowsFeature -name Web-Server -IncludeManagementTools

# Also, edit VM's webserver homepage with name of each machine so we can identify in tests
# c:\inetpub\wwwroot\iistart.html
#  ie.  <h1>ALB-PS-VM-01</h1> vs <h1>ALB-PS-VM-02</h1>

$pip1 = Get-AzureRmPublicIpAddress -Name albpsvm-ip01 -ResourceGroupName albps 
$pip2 = Get-AzureRmPublicIpAddress -Name albpsvm-ip02 -ResourceGroupName albps 

## RDP connection
mstsc /v: $pip1.IpAddress
mstsc /v: $pip2.IpAddress

# copy and install webserver tools - run powershell on windows vm
## Install-WindowsFeature -name Web-Server -IncludeManagementTools

##########################################################################

#  Create Azure Load Balancer

##########################################################################
#
# About Azure Load Balancer
# https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-overview

# Create IP for ALB (Change DomainNameLabel)
New-AzureRmPublicIpAddress -Name ALBpsPubip -ResourceGroupName albps `
-AllocationMethod Static -Location westus2 -DomainNameLabel *****************

# get ip etc # set variables
$publicIP = Get-AzureRmPublicIpAddress -Name ALBpsPubip -ResourceGroupName albps 
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP
$beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName albps -Name ALBPS-VNet

# Create the NAT rules (optional - not required for lab)
# $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389
# $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

# Create a health probe. 
# HTTP probe (two methods, using TCP in this lab)
#  $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
# TCP probe
$healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2

# Create a load balancer rule.
$lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80

# Create the load balancer by using the previously created objects.
$NRPLB = New-AzureRmLoadBalancer -ResourceGroupName albps -Name LoadBalancerPS -Location westus2 `
-FrontendIpConfiguration $frontendIP -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
# (optional ) -InboundNatRule $inboundNATRule1,$inboundNatRule2 `

$NRPLB

### Next Steps add vms/availability set to backend pools on ALB 

# Load the load balancer resource into a variable
$loadbalancer = Get-AzureRmLoadBalancer -Name LoadBalancerPS -ResourceGroupName albps
Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-Backend" -LoadBalancer $loadbalancer

# load backendpool config into variable
$backend = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-Backend" -LoadBalancer $loadbalancer

# Load the Network interface- into a variable. The variable name is $nic and $nic2
$nic = get-azurermnetworkinterface -name ALBpsvmNIC-01 -resourcegroupname albps
$nic2 = get-azurermnetworkinterface -name ALBpsvmNIC-02 -resourcegroupname albps

# set to backendpool
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
$nic2.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

#Save the network interface object.
Set-AzureRmNetworkInterface -NetworkInterface $nic
Set-AzureRmNetworkInterface -NetworkInterface $nic2


###############################################################################

# What have we created so far? 
Get-AzureRmResource | select name, resourcetype, resourcegroupname, location

# What are the IP addresses of the resources we have created?
Get-AzureRmPublicIpAddress | select name, ipaddress

# Lets test our ALB by getting IP address and open in a browser.
$albip = Get-AzureRmPublicIpAddress -Name ALBpsPubip -ResourceGroupName albps
$albip = $albip.IpAddress 

Start-Process -Filepath http://$albip

start chrome http://$albip

# To test/demo failover, stop the vm that's showing in browser, or try different browsers

#Alb VM's -  
stop-azurermvm -ResourceGroupName albps -name albpsvm-01 -force
stop-azurermvm -ResourceGroupName albps -name albpsvm-02 -force

#Alb VM's -  
start-azurermvm -ResourceGroupName albps -name albpsvm-01  
start-azurermvm -ResourceGroupName albps -name albpsvm-02 


# end Azure Load Balancer Scenario 

cls


##############################################################################
#
#       Traffic manager   
#
# Traffic manager is DNS level traffic routing, ideal for disaster recovery
# and load balancing between multiple data centers

# Traffic manager Docs
# https://docs.microsoft.com/en-us/powershell/module/azurerm.trafficmanager/?view=azurermps-5.2.0

New-AzureRMResourceGroup -name TrafficPS -location westus

########################################################

# Create Traffic Manager Profile (DNS name must be unique )

$profile = New-AzureRmTrafficManagerProfile -Name MyTrafficMgrProfile `
-ResourceGroupName TrafficPS -TrafficRoutingMethod Weighted `
-RelativeDnsName *********************** -Ttl 30 -MonitorProtocol `
HTTP -MonitorPort 80 -MonitorPath "/"

$ip1 = Get-AzureRmPublicIpAddress -Name ALBpsPubip -ResourceGroupName albps
New-AzureRmTrafficManagerEndpoint -Name ALBPS -ProfileName MyTrafficMgrProfile `
 -ResourceGroupName trafficps -Type AzureEndpoints -TargetResourceId $ip1.Id `
 -EndpointStatus Enabled

$ip2 = Get-AzureRmPublicIpAddress -Name AGWpsPubip -ResourceGroupName agwps
New-AzureRmTrafficManagerEndpoint -Name AGWPS -ProfileName MyTrafficMgrProfile `
 -ResourceGroupName trafficps -Type AzureEndpoints -TargetResourceId $ip2.Id `
 -EndpointStatus Enabled

#make sure you change subdomain name to match TM profile name in url below
start-process http://***********************.trafficmanager.net

# More about trafficmgr and powershell
# https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-powershell-arm#create-a-traffic-manager-profile
# 


# Test failover by stopping/starting VM's 
#App GW VM's -
stop-azurermvm -ResourceGroupName agwps -name agwpsvm-01 -force
stop-azurermvm -ResourceGroupName agwps -name agwpsvm-02 -force

#App GW VM's -
start-azurermvm -ResourceGroupName agwps -name agwpsvm-01  
start-azurermvm -ResourceGroupName agwps -name agwpsvm-02 

#ALB VM's - 
stop-azurermvm -ResourceGroupName albps -name albpsvm-01 -force
stop-azurermvm -ResourceGroupName albps -name albpsvm-02 -force

#ALB VM's -  
start-azurermvm -ResourceGroupName albps -name albpsvm-01  
start-azurermvm -ResourceGroupName albps -name albpsvm-02 

# Disable Traffic Manager Profile
Disable-AzureRmTrafficManagerProfile -Name MyTrafficMgrProfile -ResourceGroupName trafficps  -Force

# Enable TM
Enable-AzureRmTrafficManagerProfile -Name MyTrafficMgrProfile -ResourceGroupName trafficps


# Helpful PS cmdlet for checking VM's powerstate...
Get-AzureRmVM -Status | Select ResourceGroupName, Name, PowerState




#############################################################################

#                         notes

#############################################################################

# With powershell we have built and tested these scenarios: 
#
# resourcegroup's x3 
# nsg's x2 
# vnets + subnets
# availability sets
# AppGW 
# ALB
# VMs x 4
# Test ALB + AppGW 
# Traffic Manager 
#
# - add vm / arm template, start with portal
# - add webapp, cosmosdb
# - monitoring 
# - security
# - view resources in portal 


############## sidebar's ############################################
# The following is not part of this exercise 
# 
Get-AzureRmADUser

### if you have execution policy problems?
# execution scope policy info
Get-ExecutionPolicy -list
# to bypass execution policy
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
# set current user allsigned-force ### - great for workshop 
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy AllSigned -Force

#get resource groups
Get-AzureRmResourceGroup

# Helpful PS cmdlet for checking VM's powerstate...
Get-AzureRmVM -Status | Select ResourceGroupName, Name, PowerState

#get resources
Get-AzureRMResource 

Get-AzureRmResource | select name, kind, location
Get-AzureRmResource | select name, resourcetype, resourcegroupname,location

#get resources in specific resource group 
Get-AzureRmResource | Where-Object { $_.ResourceGroupName -eq "myjenkins"}
Get-AzureRmResource | ? { $_.Name -like "*blah*"}
#Contains Like - CLike - case sensitive plus *
Get-AzureRmResource | Where-Object { $_.ResourceGroupName -CLike "*blah*"} | select name, resourcetype

#delete resource group (and all the resources therein) 
# Remove-AzureRmResourceGroup -Name blah

Start-Process -FilePath "http://portal.azure.com" 

start-process -filepath "http://resources.azure.com"
