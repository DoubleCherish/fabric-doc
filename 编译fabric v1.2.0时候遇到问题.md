## 编译fabric v1.2.0时候遇到问题

```java
$make orderer
.build/bin/orderer
CGO_CFLAGS=" " GOBIN=/home/tom/go/src/github.com/hyperledger/fabric/.build/bin go install -tags "experimental" -ldflags "-X github.com/hyperledger/fabric/common/metadata.Version=1.2.1 -X github.com/hyperledger/fabric/common/metadata.CommitSHA=78a3a8d -X github.com/hyperledger/fabric/common/metadata.BaseVersion=0.4.10 -X github.com/hyperledger/fabric/common/metadata.BaseDockerLabel=org.hyperledger.fabric -X github.com/hyperledger/fabric/common/metadata.DockerNamespace=hyperledger -X github.com/hyperledger/fabric/common/metadata.BaseDockerNamespace=hyperledger -X github.com/hyperledger/fabric/common/metadata.Experimental=true" github.com/hyperledger/fabric/orderer
# github.com/hyperledger/fabric/vendor/github.com/docker/docker/pkg/archive
vendor/github.com/docker/docker/pkg/archive/archive.go:364:5: hdr.Format undefined (type *tar.Header has no field or method Format)
vendor/github.com/docker/docker/pkg/archive/archive.go:364:15: undefined: tar.FormatPAX
vendor/github.com/docker/docker/pkg/archive/archive.go:1166:7: hdr.Format undefined (type *tar.Header has no field or method Format)
vendor/github.com/docker/docker/pkg/archive/archive.go:1166:17: undefined: tar.FormatPAX
Makefile:256: recipe for target '.build/bin/orderer' failed
make: *** [.build/bin/orderer] Error 2

解决方法：将go语言版本升级到1.10以上
```

## 记一次启动fabric examples 中e2e_cli项目出现的错误：

```javascript
./network_setup.sh down 报如下错误：
tom@ubuntu:~/go/src/github.com/hyperledger/fabric/examples/e2e_cli$ ./network_setup.sh down
setting to default channel 'mychannel'
WARNING: The CHANNEL_NAME variable is not set. Defaulting to a blank string.
WARNING: The TIMEOUT variable is not set. Defaulting to a blank string.
Removing network e2e_default
ERROR: network e2e_default id 70ac86d56f943a2da5ffbf8b2ef7c6da30b9ba2f94369548b13017e274a5d536 has active endpoints
---- No containers available for deletion ----
---- No images available for deletion ----
./network_setup.sh up 报如下错误：
Creating peer0.org2.example.com ... error

ERROR: for peer0.org2.example.com  Cannot start service peer0.org2.example.com: endpoint with name peer0.org2.example.com already exi
Creating zookeeper2 ... error

Creating zookeeper0 ... error

Creating peer1.org2.example.com ... error

ERROR: for peer1.org2.example.com  Cannot start service peer1.org2.example.com: endpoint with name peer1.org2.example.com already exi
Creating peer1.org1.example.com ... error

ERROR: for peer1.org1.example.com  Cannot start service peer1.org1.example.com: endpoint with name peer1.org1.example.com already exi
Creating peer0.org1.example.com ... error

ERROR: for peer0.org1.example.com  Cannot start service peer0.org1.example.com: endpoint with name peer0.org1.example.com already exi
Creating zookeeper1 ... error

ERROR: for zookeeper1  Cannot start service zookeeper1: endpoint with name zookeeper1 already exists in network e2e_default

ERROR: for peer0.org2.example.com  Cannot start service peer0.org2.example.com: endpoint with name peer0.org2.example.com already exists in network e2e_default

ERROR: for peer0.org1.example.com  Cannot start service peer0.org1.example.com: endpoint with name peer0.org1.example.com already exists in network e2e_default

ERROR: for peer1.org1.example.com  Cannot start service peer1.org1.example.com: endpoint with name peer1.org1.example.com already exists in network e2e_default

ERROR: for peer1.org2.example.com  Cannot start service peer1.org2.example.com: endpoint with name peer1.org2.example.com already exists in network e2e_default

ERROR: for zookeeper0  Cannot start service zookeeper0: endpoint with name zookeeper0 already exists in network e2e_default

ERROR: for zookeeper1  Cannot start service zookeeper1: endpoint with name zookeeper1 already exists in network e2e_default

ERROR: for zookeeper2  Cannot start service zookeeper2: endpoint with name zookeeper2 already exists in network e2e_default
ERROR: Encountered errors while bringing up the project.
ERROR !!!! Unable to pull the images 


解决方案：
docker network inspect {network}  查看网络状况  例如:docker network inspect xxxxl_default

docker network disconnect -f {network} {endpoint-name}   解决办法1

sudo service docker restart  解决办法2
原因不明
```

