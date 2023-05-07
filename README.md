# CSE-534-DDOS

This repository contains the source code and instructions for setting up and running a Distributed Denial of Service Mitigation (DDOSM) system using the P4 programming language.

The DDOSM-P4 is designed to detect and mitigate DDoS attacks in real-time by filtering suspicious traffic while allowing legitimate traffic to pass through.

## Getting Started

### Prerequisites

You need to have Docker installed on your system. If you don't have Docker, you can follow the installation instructions [here](https://docs.docker.com/engine/install/).

### Setting Up

1. Clone this repository to your local machine:
git clone https://github.com/maddyrajesh/CSE-534-DDOS.git
cd ddosm-p4

2. Compile the P4 code:
docker run -it --rm -v "/home/ilha/dev/p4/ddosm-p4:/workdir" --workdir /workdir asilha/p4lang-p4c:v122ok-euclid make

3. Create Docker networks:
docker network create --driver bridge euclid_input
docker network create --driver bridge euclid_legitimate
docker network create --driver bridge euclid_suspect
docker network create --driver bridge euclid_stats
docker network ls

4. Run the DDOSM-P4 container:
docker run -dit --rm -v "/home/ilha/dev/p4/ddosm-p4:/workdir" --workdir /workdir --network euclid_input --name euclid asilha/p4lang-behavioral-model:euclid

5. Connect the container to the remaining networks:
docker network connect euclid_legitimate euclid
docker network connect euclid_suspect euclid
docker network connect euclid_stats euclid


6. Attach to the DDOSM-P4 container:
docker attach euclid

7. Run the simple_switch inside the container:
simple_switch -i 1@euclid-input -i 2@euclid-legitimate -i 3@euclid-suspect -i 4@euclid-stats build/ddosm.json &
ps w | grep simple


8. Apply the control rules to the simple_switch:
simple_switch_CLI < scripts/control_rules.txt
fg


The classifier is now up and running, monitoring traffic and mitigating DDoS attacks.

## Sources

P4 Program taken from https://github.com/aclapolli/ddosd-p4. However edits had been done to support latest versions of BMv2 and P4C.
Jupyter Notebooks, Data Preprocessing and Analysis written by author.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.

## Acknowledgements

* The P4 language team and community for providing the necessary tools and support.

