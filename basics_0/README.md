# Networking basics #0

- **[0. OSI model](./0-OSI_model)**

  - OSI (Open Systems Interconnection) is an abstract model to describe layered communication and computer network design. The idea is to segregate the different parts of what make communication possible.
  - It is organized from the lowest level to the highest level:
    - The lowest level: layer 1 which is for transmission on physical layers with electrical impulse, light or radio signal
    - The highest level: layer 7 which is for application specific communication like SNMP for emails, HTTP for your web browser, etc
  - Questions:
    - What is the OSI model?
    - How is the OSI model organized?

- **[1. Types of network](./1-types_of_network)**

  - LAN connect local devices together, WAN connects LANs together, and WANs are operating over the Internet.
  - Questions:
    - What type of network a computer in local is connected to?
    - What type of network could connect an office in one building to another office in a building a few streets away?
    - What network do you use when you browse www.google.com from your smartphone (not connected to the Wifi)?

- **[2. MAC and IP address](./2-MAC_and_IP_address)**

  - Questions:
    - What is a MAC address?
    - What is an IP address?

- **[3. UDP and TCP](./3-UDP_and_TCP)**

  - Questions:
    - Which statement is correct for the TCP box:
    - Which statement is correct for the UDP box:
    - Which statement is correct for the TCP worker:

- **[4. TCP and UDP ports](./4-TCP_and_UDP_ports)**

  - Write a Bash script that displays listening ports:
    - That only shows listening sockets
    - That shows the PID and name of the program to which each socket belongs

- **[5. Is the host on the network](./5-is_the_host_on_the_network)**

  - Write a Bash script that pings an IP address passed as an argument.
  - Requirements:
    - Accepts a string as an argument
    - Displays `Usage: 5-is_the_host_on_the_network {IP_ADDRESS}` if no argument passed
    - Ping the IP 5 times
