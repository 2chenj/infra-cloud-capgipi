# infra-cloud-capgipi

## NIC : Network Interfaces ( Thierry / Florian / Solal) : 172.16.0.0/29 ,255.255.255.248
> Subnet
> NSG : Network Security Group
> PIP : Public IP ( Thierry / Florian / Solal)
> VNET : Virtual Network  
> RG : Ressources Groupe 

nom du ressource groupe: Capipi_Resource
security group: Capipi_Security
Pour azure_rm_publicipaddress ça sera "Capipi_ip_public"

#composition de l'équipes:
florian
alex
léo
adem
ousmane
quentin
thierry

#Définition du playbook

- name: Create resource group
  azure_rm_resourcegroup:
    name: Capipi_Resource
    location: uksouth
- name: Creation réseau virtuel
  azure_rm_virtualnetwork:
    resource_group: Capipi_Resource
    name: Capipi_Vnet
    address_prefixes: "172.16.0.0/29"
- name: Ajouter un sous réseau
  azure_rm_subnet:
    resource_group: Capipi_Resource
    name: Capipi_Subnet
    address_prefix: "172.16.0.0/29"
    virtual_network: Capipi_Vnet
- name: Capipi_ip_public
  azure_rm_publicipaddress:
    resource_group: Capipi_Resource
    allocation_method: Static
    name: Capipi_ip_public
- name: Create Network Security Group that allows SSH
  azure_rm_securitygroup:
    resource_group: Capipi_Resource
    name: Capipi_Security
    rules:
      - name: SSH
        protocol: Tcp
        destination_port_range: 22
        access: Allow
        priority: 122
        direction: Inbound
- name: Capipi_Vnet
  azure_rm_networkinterface:
    resource_group: Capipi_Resource
    name: Capipi_NIC
    virtual_network: 172.16.0.0
    subnet: 255.255.255.248
    public_ip_name: Capipi_ip_public
    security_group: Capipi_Security
