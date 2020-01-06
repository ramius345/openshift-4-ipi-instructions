# openshift-4-ipi-instructions
# Download the latest AWS Command Line Interface
curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
unzip awscli-bundle.zip
# Install the AWS CLI into /bin/aws
./awscli-bundle/install -i /usr/local/aws -b /bin/aws
# Validate that the AWS CLI works
aws --version
# Clean up downloaded files
rm -rf /root/awscli-bundle /root/awscli-bundle.zip




OCP_VERSION=4.2.13

#Get openshift-install tool
wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/${OCP_VERSION}/openshift-install-linux-${OCP_VERSION}.tar.gz
tar zxvf openshift-install-linux-${OCP_VERSION}.tar.gz -C /usr/bin
rm -f openshift-install-linux-${OCP_VERSION}.tar.gz /usr/bin/README.md
chmod +x /usr/bin/openshift-install
#Get OC tool
wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/${OCP_VERSION}/openshift-client-linux-${OCP_VERSION}.tar.gz
tar zxvf openshift-client-linux-${OCP_VERSION}.tar.gz -C /usr/bin
rm -f openshift-client-linux-${OCP_VERSION}.tar.gz /usr/bin/README.md
chmod +x /usr/bin/oc



export AWSKEY=<YOURACCESSKEY>
export AWSSECRETKEY=<YOURSECRETKEY>
export REGION=<YOURREGION>
echo "export REGION=${REGION}" >> $HOME/.bashrc:
mkdir $HOME/.aws
cat << EOF >> $HOME/.aws/credentials
[default]
aws_access_key_id = ${AWSKEY}
aws_secret_access_key = ${AWSSECRETKEY}
region = $REGION
EOF


[tpetersh-redhat.com@clientvm 0 ~]$ aws sts get-caller-identity
{
    "Account": "783499926113",
    "UserId": "AIDA3M3B4DJQR37URU2GX",
    "Arn": "arn:aws:iam::783499926113:user/tpetersh@redhat.com-2915"
}
[tpetersh-redhat.com@clientvm 0 ~]$

If you only have cli access, give yourself console access.  First find your username:
[tpetersh-redhat.com@clientvm 0 ~]$ aws iam get-user
{
    "User": {
        "UserName": "tpetersh@redhat.com-2915",
        "Tags": [
    ….


Go to https://cloud.openshift.com/ and log in with client credentials.

Click on “Create Cluster”

Click on “Openshift Container Platform”

Click on “AWS”

Select “Installer-Provisioned Infrastructure”

Scroll down and select “Copy Pull Secret”.  Save the pull secret for use with the installer.

Generate an SSH Key:

ssh-keygen -f ~/.ssh/cluster-<NAME>-key -N ''

Run the openshift installer

openshift-install create cluster --dir $HOME/cluster-${GUID}
? SSH Public Key /home/<OpenTLC User>/.ssh/cluster-<GUID>-key.pub
? Platform aws
? Region us-east-2 (Ohio)
? Base Domain sandboxNNN.opentlc.com
? Cluster Name cluster-<name>
? Pull Secret
******************************************************************************


Setup the oc command to system:admin by running the following:
export KUBECONFIG=$HOME/cluster-<name>/auth/kubeconfig
oc whoami
