测试更新链
cd /Users/zuiyou/zuiyou_work/blog/cmd/blogd
go build -ldflags="-X github.com/cosmos/cosmos-sdk/version.Version=v0.0.1" -o blogd main.go
rm -rf ~/.blog
./blogd config chain-id blog
./blogd config keyring-backend test
./blogd config broadcast-mode sync
./blogd init blog --chain-id blog --overwrite
cat <<< $(jq '.app_state.gov.voting_params.voting_period = "20s"' $HOME/.blog/config/genesis.json) > $HOME/.blog/config/genesis.json


./blogd keys add validator
./blogd add-genesis-account validator 1000000000stake --keyring-backend test
./blogd gentx validator 1000000stake --chain-id blog
./blogd collect-gentxs


export DAEMON_NAME=blogd
export DAEMON_HOME=$HOME/.blog
export DAEMON_RESTART_AFTER_UPGRADE=true

mkdir -p $DAEMON_HOME/cosmovisor/genesis/bin
cp ./blogd $DAEMON_HOME/cosmovisor/genesis/bin


cosmovisor run start




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
