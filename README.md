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

### 2. Auto-Generation: Setting up the Starter Workflow
To make GitHub suggest and "auto-generate" your workflow for users, follow these steps in your repository:

1. Create a folder named `.github/workflow-templates` at the root of your action repo.
2. Inside that folder, create `cloudpanel-deploy.yml`:
   ```yaml
   name: CloudPanel Deploy
   on:
     push:
       branches: [ $default-branch ]
   jobs:
     deploy:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v4
         - uses: amjadiqbal/cloudpanel-deploy-action@v1
           with:
             host: ${{ secrets.CLOUDPANEL_HOST }}
             user: 'site-user'
             key: ${{ secrets.CLOUDPANEL_SSH_KEY }}
             working_dir: '/home/site-user/htdocs/example.com'
   ```
3. Create a metadata file cloudpanel-deploy.properties.json in the same folder:
   ```json
   {
     "name": "CloudPanel Deployment",
     "description": "Automatically deploy your code to a CloudPanel server.",
     "iconName": "cloud",
     "categories": ["Deployment"]
   }
   ```
