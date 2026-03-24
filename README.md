# CloudPanel Deploy Action 🚀

Deploy to your [CloudPanel](https://www.cloudpanel.io) sites instantly. This action automates the SSH connection, directory navigation, and execution of your deployment scripts.

## 🛠 Setup in 2 Minutes

1. **Add Secrets**: In your GitHub Repo, go to `Settings > Secrets` and add:
   - `CLOUDPANEL_HOST`: Your server IP address.
   - `CLOUDPANEL_SSH_KEY`: Your private SSH key.
2. **Install**: Go to the **Actions** tab in your repo, search for "CloudPanel Deploy", and click **Configure**. This will auto-generate your workflow file.

## ⚙️ Configuration (Inputs)


| Input | Description | Required | Default |
| :--- | :--- | :---: | :--- |
| `host` | CloudPanel Server IP | Yes | - |
| `user` | CloudPanel Site User | Yes | - |
| `working_dir` | Path to site (e.g. `/home/user/htdocs/site.com`) | Yes | - |
| `script` | **Custom deployment commands** | No | `git pull origin main` |

## How It Works (Rsync Method) 🛠
Unlike standard git-based tools, this action uses **rsync** to sync your code. 

- **No Server-Side Git Required**: You don't need to run `git clone` or `git pull` on your CloudPanel server.
- **Safe Merging**: It copies new/updated files from GitHub and merges them into your server folder. 
- **Non-Destructive**: It will **not** delete existing files on your server (like user uploads or local logs) unless you explicitly configure it to do so.
  
### Example Workflow
```yaml
- name: CloudPanel Deploy
  uses: amjadiqbal/cloudpanel-deploy-action@v1.2.0
  with:
    host: ${{ secrets.CP_HOST }}
    user: 'my-site-user'
    key: ${{ secrets.CP_KEY }}
    working_dir: '/home/my-site-user/htdocs/://site.com'
    script: |
      npm install
      npm run build
```

### Customizing Your Script
You can extend the `script` input to run any commands specific to your app:
```yaml
- uses: amjadiqbal/cloudpanel-deploy-action@v1
  with:
    host: ${{ secrets.CLOUDPANEL_HOST }}
    script: |
      git pull origin main
      composer install --no-dev
      php artisan migrate --force
      php artisan optimize
```
