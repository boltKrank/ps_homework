#Compile master install:

From the node:
`curl -k https://<MoM_HOSTNAME>:8140/packages/current/install.bash | sudo bash -s main:dns_alt_names=<COMMA-SEPARATED LIST OF ALT NAMES FOR THE COMPILE MASTER>`

From the master:
`puppet cert --allow-dns-alt-names sign <COMPILE_MASTER_HOSTNAME>`

#Setting up load balaner (nginx):


#Configure the MoMs as AMQ Hubs

#Configure the Compile Masters as AMQ brokers
