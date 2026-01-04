# Adding Insecure Registries in Rancher Desktop Lima

* **Enter Lima VM shell:** `rdctl shell`
* **View and edit Docker config:** `sudo vi /etc/docker/daemon.json`

```json
{
  "insecure-registries": [
    "localhost:8082"
  ],
  "features": {
    "containerd-snapshotter": false
  }
}
````

* **Restart Docker to apply changes:** `sudo service docker restart`

* **Exit Lima VM shell:** `exit`

* **To add a host later in one line (replace <YOUR_DOCKER_HOST>):**

```bash
rdctl shell -- sudo sh -c 'sed -i "/^\s*]/i\    \"<YOUR_DOCKER_HOST>\"," /etc/docker/daemon.json; sudo service docker restart'
```
