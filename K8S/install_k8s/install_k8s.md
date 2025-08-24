# add node
sudo kubeadm token list
sudo kubeadm join --token <token> <control-plane-host>:<control-plane-port> 

# multi-node machine

## When:

Depends. What are the specs of your machine? If it's something small, then no benefits. If it's something beefy, then it makes sense.

If you have a situation where you have to reboot the machine (kernel update, zombie process, ...). If you now have a massive node with tons of Pods / containers, that might take ages to reschedule. If your node is now split into smaller VMs, the scale and impact of that operation is reduced dramatically.

or for testing purpose. the best way to do it so is to use kind . 

# one-node machine

## When :
best pratice use only one worker node by VM/Machine.


