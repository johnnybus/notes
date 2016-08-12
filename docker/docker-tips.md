# Docker Quick Tips
##### Export env to work correctly with compose
    eval $(docker-machine env default)
##### If some containers where removed without -v option, which means, that the volumes created by those containers will persist on the host machine, we can remove them by:
    docker volume rm $(docker volume ls -qf dangling=true)
##### Run command on a running docker container
    docker exec <container id> <command>
##### Docker machine example with proxy on Parallels and VMware fusion
    docker-machine create -d parallels \ 
        --engine-opt dns=172.31.130.7 \ 
        --parallels-disk-size=10240 \ 
        --parallels-memory=256 \ 
        --engine-env http_proxy=http://<proxy-url>:<port> \ 
        --engine-env https_proxy=[http://<proxy-url>:<port>\ 
        <machine-name> 

    docker-machine create -d vmwarefusion \ 
        --engine-opt dns=172.31.130.7 \ 
        --vmwarefusion-disk-size=10240 \ 
        --vmwarefusion-memory-size=1024 \ 
        --engine-env http_proxy=http://<proxy-url>:<port> \ 
        --engine-env https_proxy=[http://<proxy-url>:<port>\ 
        cgi 
