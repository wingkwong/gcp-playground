# SSH Keys

## Generating SSH Keys

```
ssh-keygen -t rsa -f ~/.ssh/[KEY_FILENAME] -C [USERNAME]
```

Restrict access 
```
chmod 400 ~/.ssh/[KEY_FILENAME]
```

## Locating an SSH Key

```
Linux and macOS
Public key: $HOME/.ssh/google_compute_engine.pub
Private key: $HOME/.ssh/google_compute_engine

Windows:
Public key: C:\Users\[USERNAME]\.ssh\google_compute_engine.pub
Private key: C:\Users\[USERNAME]\.ssh\google_compute_engine
```

## Adding/Removing Project-Wide Public SSH Keys

```
gcloud compute project-info describe
```

```
...
metadata:
  fingerprint: QCofVTHlggs=
  items:
  - key: ssh-keys
    value: |-
      [USERNAME_1]:ssh-rsa [EXISTING_KEY_VALUE_1] [USERNAME_1]
      [USERNAME_2]:ssh-rsa [EXISTING_KEY_VALUE_2] [USERNAME_2]
...
```

Project Level

```
gcloud compute project-info add-metadata --metadata-from-file ssh-keys=[LIST_PATH]
```

Instance Level

```
gcloud compute instances add-metadata [INSTANCE_NAME] --metadata-from-file ssh-keys=[LIST_PATH]
```

## Blocking Project-Wide Public SSH Keys

```
gcloud compute instances add-metadata [INSTANCE_NAME] --metadata block-project-ssh-keys=TRUE
```