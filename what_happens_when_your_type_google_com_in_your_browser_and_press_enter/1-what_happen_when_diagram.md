# Architectural Diagram: What Happens When You Type https://www.google.com in Your Browser

This diagram illustrates the step-by-step infrastructure and network flow behind a standard HTTPS request to Google's infrastructure.

## Diagram

```mermaid
graph TD
    %% Style & Configuration
    subgraph Client_Side [Client Side]
        A[User Browser]
    end

    subgraph Network_Zone [Network & Security]
        B((1. DNS Resolution<br>Domain to IP))
        C{3. Firewall<br>Packet Filtering}
    end

    subgraph Google_Infrastructure [Google Infrastructure]
        D[4. Load Balancer<br>Traffic Distribution]
        
        subgraph Server_Cluster [Backend Stack]
            E[5. Web Server<br>Nginx / Apache]
            F[6. Application Server<br>Business Logic]
            G[(7. Database<br>Google Spanner / Clusters)]
        end
    end

    %% Request Flow
    A -->|User types URL| B
    B -->|Returns IP Address| A
    
    A ==>|2. HTTPS Request via Port 443<br>SSL/TLS Encrypted Tunnel| C
    
    C ==>|Secure and Allowed Traffic| D
    
    D ==>|Traffic Distribution| E
    E ==>|Dynamic Request| F
    
    %% Database Interaction
    F -->|Query Data| G
    G -->|Return Data| F
    
    %% Response Flow
    F -->|Generates HTML/CSS/JS Page| E
    E -->|HTTP Response| D
    D ==>|Encrypted Stream| C
    C ==>|Renders Homepage| A

    %% Visual Styles
    style A fill:#ea4335,stroke:#333,stroke-width:2px,color:#fff
    style B fill:#fabc05,stroke:#333,stroke-width:1px
    style C fill:#4285f4,stroke:#333,stroke-width:2px,color:#fff
    style D fill:#34a853,stroke:#333,stroke-width:2px,color:#fff
    style G fill:#9334e6,stroke:#333,stroke-width:2px,color:#fff
    
    linkStyle 2 stroke:#34a853,stroke-width:3px;
    linkStyle 3 stroke:#34a853,stroke-width:3px;
    linkStyle 4 stroke:#34a853,stroke-width:3px;
    linkStyle 10 stroke:#ea4335,stroke-width:3px;
```

## Architectural Workflow Breakdown

1. **DNS Resolution:** The Client browser requests the IP address for `www.google.com` via Local, Resolver, and Authoritative DNS servers.
2. **Firewall & Port Check:** Traffic travels over the internet to the destination IP address, hitting **Port 443** (standard for HTTPS). It passes through a security network **Firewall** which monitors and filters incoming packets.
3. **Encrypted Traffic (SSL/TLS):** A secure handshake is performed, establishing an **encrypted SSL/TLS tunnel** between the Client and the infrastructure.
4. **Load Balancing:** The encrypted request hits the **Load Balancer**, which acts as a traffic manager and distributes the load across healthy backend servers.
5. **Web Server Processing:** The **Web Server** decrypts/handles the HTTP request. If the request requires dynamic processing, it passes it down the line.
6. **Application Server Logic:** The **Application Server** executes the backend code (business logic) to build the dynamic components of the page.
7. **Database Querying:** The Application Server requests and pulls the necessary data (user preferences, search indexes) from the distributed **Database** cluster.
8. **The Response Loop:** The Database returns data ➔ Application Server generates the HTML/CSS/JS page ➔ Web Server handles the response ➔ Load Balancer routes it back ➔ Encrypted traffic passes the Firewall ➔ Client browser renders the page.
