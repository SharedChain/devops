# Configure 


## Edit local SSH config
```vi ~/.ssh/config```

Add:
```
Host bastion.ultri.net
  ForwardAgent yes
```

## Check thatlocal key agent is running
```
echo "$SSH_AUTH_SOCK"
```

## List the keys in your ring
```
ssh-add -L
```

## Add a new key temporarily
```
ssh-add ~/.ssh/key-pair-useast.pem
```
-- or Keep it, add it permanently --
```
ssh-add -K ~/.ssh/key-pair-useast.pem
```