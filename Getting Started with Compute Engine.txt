# Task 2: Create a virtual machine using the GCP Console
# In the GCP console type the following or copy and paste it into your console and execute the following command:

gcloud beta compute --project=qwiklabs-gcp-04-816963388372 instances create my-vm-1 --zone=us-central1-a --machine-type=e2-medium --subnet=default --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=823034820350-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --tags=http-server --image=debian-9-stretch-v20200902 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=my-vm-1 --reservation-affinity=any

# This will create an instance named my-vm-1 in the Region and Zone us-central1-a, a machine type of e2-medium in the default subnet, with a boot disk size of 10GB, and an image of Debian GNU/Linux 9 (stretch) .

# To create a Firewall that will allow HTTP traffic execute this command:

gcloud compute firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server

# Note: The VM can take about two minutes to launch and be fully available for use.


# Task 3: Create a virtual machine using the gcloud command line
# To display a list of all the zones in the region to which Qwiklabs assigned you, enter this partial command:  gcloud compute zones list | grep followed by the region us central1.
# Your completed command will look like this:

gcloud compute zones list | grep us-central1

# Choose a zone from that list other than the region us-central1 and zone us-central1-a e.g you might choose zone us-central1-b.
# To set your default zone to the one you just chose, enter this partial command gcloud config set compute/zone followed by the zone you chose.
# Your completed command will look like this:

gcloud config set compute/zone us-central1-b

# To create a VM instance called my-vm-2 in that zone, execute this command:

gcloud compute instances create "my-vm-2" \
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image "debian-9-stretch-v20190213" \
--subnet "default"

# Note: The VM can take about two minutes to launch and be fully available for use.
# To close the Cloud Shell, execute the following command:

exit


# Task 4: Connect between VM instances
# In gcloud console execute the following command to list all the instances you have created:

gcloud compute instances list

# You will see the two VM instances you created, each in a different zone.
# Notice that the Internal IP addresses of these two instances share the first three bytes in common. They reside on the same subnet in their Google Cloud VPC even though they are in different zones.
# To open an SSH connection to the my-vm-2 instance, execute the follwing command:

gcloud compute ssh my-vm-2

# Accept any prevailing message/s with a Y for yes
# You will now be connected to the my-vm-2
# USe the ping command to confirm that my-vm-2 can reach my-vm-1 over the network:

ping my-vm-1

# Notice that the output of the ping command reveals that the complete hostname of my-vm-1 is my-vm-1.c.PROJECT_ID.internal, where PROJECT_ID is the name of your Google Cloud Platform project. GCP automatically supplies Domain Name Service (DNS) resolution for the internal IP addresses of VM instances.

# Press Ctrl+C to abort the ping command.
#Use the ssh command to open a command prompt on my-vm-1:

ssh my-vm-1

# If you are prompted about whether you want to continue connecting to a host with unknown authenticity, enter yes to confirm that you do.
#At the command prompt on my-vm-1, install the Nginx web server:

sudo apt-get install nginx-light -y

# Use the nano text editor to add a custom message to the home page of the web server:

sudo nano /var/www/html/index.nginx-debian.html

# Use the arrow keys to move the cursor to the line just below the h1 header. Add text like this, and replace YOUR_NAME with your name:

Hi from YOUR_NAME

# Press Ctrl+O and then press Enter to save your edited file, and then press Ctrl+X to exit the nano text editor.

# Confirm that the web server is serving your new page. At the command prompt on my-vm-1, execute this command:

curl http://localhost/

# The response will be the HTML source of the web servers home page, including your line of custom text.

# To exit the command prompt on my-vm-1, execute this command:

exit

# You will return to the command prompt on my-vm-2

# To confirm that my-vm-2 can reach the web server on my-vm-1, at the command prompt on my-vm-2, execute this command:

curl http://my-vm-1/

# The response will again be the HTML source of the web servers home page, including your line of custom text.
