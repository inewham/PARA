# Kasm workspace

[Single Server Upgrade — Kasm 1.14.0 documentation](https://kasmweb.com/docs/latest/upgrade/single_server_upgrade.html)

- Move to temporary directory:

```jsx
cd /tmp
```

- Download Kasm workspace package:

```bash
curl -O https://kasm-static-content.s3.amazonaws.com/kasm_release_1.14.0.3a7abb.tar.gz
```

- Extract Kasm workspace package:

```bash
tar -xf kasm_release_1.14.0.3a7abb.tar.gz
```

Execute installation script: 

```bash
sudo bash kasm_release/install.sh
```