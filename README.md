# ansible repo

## Create vault encrypted file

``` shell
./bin/run ansible-vault create <filename>
```

## Running with ansible-vault

- Prompting for vault password
``` shell
./bin/run ansible-playbook --ask-vault-pass playbook.yml
```

- Supplying vault password via file

``` shell
./bin/run ansible-playbook --vault-password-file=/ansible/.vault_pass playbook.yml
```
