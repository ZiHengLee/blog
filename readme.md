测试更新链
cd /Users/zuiyou/zuiyou_work/blog/cmd/blogd
go build -o blogd main.go
./blogd init node --home="/Users/zuiyou/zuiyou_work/blog/cmd/blogd/node"

./blogd keys add alice --home="/Users/zuiyou/zuiyou_work/blog/cmd/blogd/node" --keyring-backend test
./blogd keys add jack --home="/Users/zuiyou/zuiyou_work/blog/cmd/blogd/node" --keyring-backend test

./blogd add-genesis-account alice 100000000stake --home="/Users/zuiyou/zuiyou_work/blog/cmd/blogd/node" --keyring-backend test
./blogd add-genesis-account jack  100000000stake --home="/Users/zuiyou/zuiyou_work/blog/cmd/blogd/node" --keyring-backend test

./blogd gentx jack 90000000stake --home="/Users/zuiyou/zuiyou_work/blog/cmd/blogd/node" --keyring-backend test
./blogd  collect-gentxs --home="/Users/zuiyou/zuiyou_work/blog/cmd/blogd/node"

初始化cosmoviser
export DAEMON_NAME=blogd
export DAEMON_HOME=/Users/zuiyou/zuiyou_work/blog/cmd/blogd/node
export DAEMON_RESTART_AFTER_UPGRADE=true

./cosmovisor run start --home /Users/zuiyou/zuiyou_work/blog/cmd/blogd/node
