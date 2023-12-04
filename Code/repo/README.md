# Installing the Clockwork Repository

First, you will need to copy the file located in this folder titled `clockworkpi.asc` to the devices `/etc/apt/trusted.gpg.d/` folder like so:

```
sudo cp ./clockworkpi.asc /etc/apt/trusted.gpg.d/.
```

Next, you will need to copy the repository list associated with the key. This file is also located in this folder, titled `clockworkpi.list`. This file must be copied to `/etc/apt/sources.list.d/`.

```
sudo cp ./clockworkpi.list /etc/apt/sources.list.d/.
```

Finally, an `apt update` will get you to where you need to be to install `devterm*` and `uconsole*` based packaged.

```
sudo apt update
```

## TLDR
```
sudo cp ./clockworkpi.asc /etc/apt/trusted.gpg.d/.
sudo cp ./clockworkpi.list /etc/apt/sources.list.d/.
sudo apt update
```
