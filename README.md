# SSH Signatures

A playground based on [this SuperUser answer](https://superuser.com/a/1616656) about ssh signatures with `ssh-keygen`.

## Create a Private/Public Key-Pair

```
ssh-keygen -f ./id_rsa -t rsa -b 4096
```

## Sign a message

```
ssh-keygen -Y sign -n file -f id_rsa content.txt
```

## Verify a message

```
cat content.txt | ssh-keygen -Y check-novalidate -n file -s content.txt.sig
```

```
Good "file" signature with RSA key SHA256:1mTk52X83F69zQ6c/Tu+71rTkqCG3/jO3sWi1pijHt4
```

### Signed by a rogue signer.

```
cat content.txt | ssh-keygen -Y check-novalidate -n file -s content.txt.rogue.sig
```

```
Good "file" signature with RSA key SHA256:71Dk1+wxY6uImSltKB/11vU/bJyTg+RVqmJTdUR25Aw
```

## Verify a tampered message

```
cat content.tampered.txt | ssh-keygen -Y check-novalidate -n file -s content.txt.sig
```

```
Signature verification failed: incorrect signature
Could not verify signature.
```

## Verify allowed signers

```
cat content.txt | ssh-keygen -Y verify -n file -f allowed_signers -s content.txt.sig -I jesse@local
```

```
Good "file" signature for jesse@local with RSA key SHA256:1mTk52X83F69zQ6c/Tu+71rTkqCG3/jO3sWi1pijHt4
```

```
cat content.txt | ssh-keygen -Y verify -n file -f allowed_signers -s content.txt.rogue.sig -I jesse@local
```

```
Could not verify signature.
```
