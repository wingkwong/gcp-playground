# Customer-supplied encryption keys (CSEK)

## Create CSEK 

Create an AES-256 base-64 key.

```python
python3 -c 'import base64; import os; print(base64.encodebytes(os.urandom(32)))'
```

Example Result

```
b'tmxElCaabWvJqR7uXEWQF39DhWTcDvChzuCmpHe6sb0=\n'
```

Create a boto file

```
vi .boto
```

> A boto config file is a text file formatted like an . ini configuration file that specifies values for options that control the behavior of the boto library. In Unix/Linux systems, on startup, the boto library looks for configuration files in the following locations and in the following order: /etc/boto.

> If the .boto file is empty, close the nano editor with Ctrl+X and generate a new .boto file using the gsutil config -n command. Then, try opening the file again with the above commands.

> If the .boto file is still empty, you might have to locate it using the gsutil version -l command.

Copy the value of the generated key excluding b' and \n' from the command output and paste it into boto file.

```
encryption_key=tmxElCaabWvJqR7uXEWQF39DhWTcDvChzuCmpHe6sb0=
```

Save the file.

## Rotate CESK keys

```
vi .boto
```

```
encryption_key = [NEW_ENCRYPTION_KEY]
decryption_key1 = [OLD_ENCRYPTION_KEY]
```

Use ``gsutil rewrite`` with ``-k`` flag

```
gsutil rewrite -k gs://[BUCKET_NAME]/[OBJECT_NAME]
```