# Add an ssh key to the ssh-agent

## List keys in agent
```ssh-add -L```

## Add key to agent
```ssh-add ~/.ssh/your-key-pair-useast.pem```

## Verify it' there
ssh-add -L

## Forward tfaffic from bastion

Add the following to the `~/.ssh/config`
```Host bastion.ultri.net
  ForwardAgent yes```


