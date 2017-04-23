Intermediate Two: REST APIs
Some customers want to interact with Puppet via the REST APIs to perform numerous provisioning and de-provisioning tasks.
Investigate the following, from the perspective of a separate provisioning node (i.e. not a Puppet Master):
Security required for the provisioning node to communicate to the various REST APIs.
Create a new Node Group, obtaining the ID of new Node Group at time of creation.
Add a rule for two nodes to the new Now Group.
Add two classes, each with at least two parameters, to the new Node Group.
Add another node to the new Node Group.
Add another class, with at least two parameters, to the new Node Group.
Spin up a new node that matches at least one of the rules created, sign the CSR via one of the APIs and ensure the node is auto-classified after signing.
Write a report containing information about:
security.
required and optional keys for each interaction with the API.
limitations of the APIs.
Include your code for each step.
 
