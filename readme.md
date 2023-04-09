# SETUP_HTTPS_LOAD_BALANCER_GCP

1. Setup instance template, you can setup instance template on instace grup, in this tutorial we will setup instance template on instance grup

2. open instance grup on compute engine 
    - clik create instance grup
    - fill in the name (is up to you)
    - defscription empty
    - klik instance template 
    - choose name instance template
        if you not yet make instance template, you can make instance template
        - clik Create an instance template
        - fill in the name (is up to you)
        - choose series machine is up to you (skip some item, continue to the following step)
        - clik Advanced options at the bottom
        - clik network 
        - fill in network tags (remember name tag for firewall)
        - network interfaces (default)
        - fill automation with the scrip

        sudo apt update
        sudo curl -fsSL https://deb.nodesource.com/setup_19.x | bash - && apt-get install -y nodejs
        sudo apt-get install git -y
        sudo git clone https://github.com/sepol-sys/express_js.git /home/c065dsx0772/web/express_js && cd /home/c065dsx0772/web/express_js
        sudo npm install
        npm run start
        
        - clik save and continue

    - choose single zone (zone is up to you)
    - fill minimun number ogf instance 2
    - fill maximun number ogf instance 5
    - add port mapping 
        - name port http 
        - port number (port service your web/app, my service is 3000)
    
    - clik create

3. create firewall for instance
    - firewall on vpc network
    - clik firewall 
    - clik Create a firewall rule
    - fill in the name of the firewall is up to you
    - fill name tags with name tags which was previously used in instance template
    - fill source ipv4 with 0.0.0.0/0
    - check TCP fill port with 3000(port service your web/app)
    - clik create

4. test connection on of instance
    - copy ip external one of instance 
    - open browser paste ip external by port (23.54.156.1:300)
    - your web is up and running

5. create load balancer
    - clik Create a load balancer
    - choose HTTP(S) and start configure
    - choose From Internet to my VMs or serverless services and Global HTTP(S) Load Balancer
    - fill in the name of the load balancer is up to you
    - clik frondend configure
        - fill in the name of the frontend
        - protocol choose HTTPS
        - IPV4 (create static ipv4 address)
            - clik ip address
            - clik create ip addrees
            - fill in the anme of static ip address
            - clik reserve
            - port 443
            - certificate
                - clik name certifikate
                    - if you don't have certificate you can clik create a new certificate
                    - fill in the name of the certificate is up to you
                    - choose Create Google-managed certificate 
                    - fill your domain, make sure you have resgiter domain well it's on cloud domain(GCP) or platfrom other  
                        - if you have certificate at ohter platform you can choonse upload my certificate and fill the certificate
                -   clik create for certificate

    - click Backend configuration
    - choose name backend if you have
        - if you don;t have backend service or bucket you can do it first
        - clik create a backend service 
        - fill in the name of the name backend service
        - backend type choose instance group
        - protocol http, name port http 
        - choose instance group which was previously craete at instanc group
        - port number fit with service port you web/app
        - clik done
        - unchek enable cloud cdn
        - clik health check
        - create a health check
            - fill in the name of the name health check 
            - protocol TCp, port number fit with service port you web/app
        - save for health check
    - create for backend service
    - skip routing rules
    - clik create for load balancer
    - clik name load balancer, clik name certificete
    - don't forget add record you sub domain as domain you load balacer
    - if status still PROVISIONING, you need 20-30 minute to status aktif



