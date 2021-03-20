# Set Up the GCP Compute Engine VM Instance

1. Click on **SSH** next to your instance.
<img width="849" alt="image" src="https://user-images.githubusercontent.com/44251159/111886784-6e1a7d80-899e-11eb-9c5c-0602915c5346.png">

2. Click on **Connect**.
<img width="488" alt="image" src="https://user-images.githubusercontent.com/44251159/111886794-87bbc500-899e-11eb-9916-ca6ea40b6684.png">

3. Install python on the VM.

```
sudo apt update
sudo apt install python3 python3-dev python3-venv
```

4. Install pip on the VM.

```
sudo apt install wget
wget https://bootstrap.pypa.io/get-pip.py
sudo python3 get-pip.py
```

5. Verify that pip has been installed.

```
pip --version
```
