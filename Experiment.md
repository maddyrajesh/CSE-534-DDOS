# DDoS Mitigation Experiment

In this experiment, we will set up a virtual environment, run a P4-based DDoS mitigation application, apply control rules, and replay a sample DDoS dataset to observe the application's behavior. 

## Steps

### 1. Set up the virtual environment
Run the `run_veth.sh` script to set up a virtual environment with 8 virtual Ethernet interfaces:

sudo ./scripts/archive/run_veth.sh setup 8

### 2. Start the simple_switch application
Run the simple_switch application with the ddosm.json P4 program, and bind the virtual Ethernet interfaces to the switch:

sudo simple_switch --log-level debug -i 1@veth0 -i 2@veth2 -i 3@veth4 -i 4@veth6 build/ddosm.json &

### 3. Apply control rules
Load the control rules from the control_rules.txt file using the simple_switch_CLI tool:

sudo simple_switch_CLI < /home/ubuntu/ddosm-p4/scripts/control_rules.txt

### 4. Set the mitigation threshold
Set the mitigation threshold for the application by writing the value 10 to the mitigation_t register:

echo "register_write mitigation_t 0 10" | simple_switch_CLI

### 5. Start packet capture
Start capturing packets to output files in the /home/ubuntu/ddosm-p4/output directory:

sudo ./scripts/archive/run_capture_to_files.sh start /home/ubuntu/ddosm-p4/output

### 6. Replay the DDoS dataset
Replay the DDoS dataset from the ddos20-full.pcap file using tcpreplay:

sudo tcpreplay -i veth0 /home/ubuntu/ddosm-p4/datasets/synthetic-ilha-ddos20-full/ddos20-full.pcap 2>&1

### 7. Stop packet capture
Stop packet capture

sudo ./scripts/archive/run_capture_to_files.sh stop /home/ubuntu/ddosm-p4/output

