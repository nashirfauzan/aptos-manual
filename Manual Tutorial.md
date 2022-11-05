# Manual Installation for Aptos Node

If you want to setup fullnode manually follow the steps below

## 1. Update packages
```
sudo apt update && sudo apt upgrade -y
```

## 2. Install docker
```
sudo apt-get install ca-certificates curl gnupg lsb-release -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io -y
```

## 3. Download configs
```
mkdir aptos-fullnode && cd aptos-fullnode
mkdir data && \
curl -O https://raw.githubusercontent.com/aptos-labs/aptos-core/devnet/config/src/config/test_data/public_full_node.yaml && \
curl -O https://devnet.aptoslabs.com/waypoint.txt && \
curl -O https://devnet.aptoslabs.com/genesis.blob
```

## 4. Start application
```
docker run --pull=always -d -p 8080:8080 -p 9101:9101 -v $(pwd):/opt/aptos/etc -v $(pwd)/data:/opt/aptos/data --workdir /opt/aptos/etc --name=aptos-fullnode aptoslabs/validator:devnet aptos-node -f /opt/aptos/etc/public_full_node.yaml
```

## Useful Commands

### Check logs
```
docker logs -f aptos-fullnode --tail 50
```

### Check sync status
```
curl 127.0.0.1:9101/metrics 2> /dev/null | grep aptos_state_sync_version | grep type
```

### Restart service
```
docker restart aptos-fullnode
```

### Delete node
```
docker stop aptos-fullnode
docker rm aptos-fullnode -f
docker volume rm aptos-fullnode -f
cd $HOME && rm aptos-fullnode -rf
```

## Full-Node Manual from Aptos dev
If u looking for manual aptos you can visit this link
https://aptos.dev/nodes/full-node/public-fullnode/
<blockquote>
<p dir="auto"><a href="https://aptos.dev/tutorials/run-a-fullnode" rel="nofollow">Run Full Node from Aptos Dev</a></p>
</blockquote>
