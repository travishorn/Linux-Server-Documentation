# Linux Server

## Serve a Next.js App

### Prerequisites

- Node.js is installed
- The Next.js app's source code is accessible in a git repository

### Clone the repository

Use `git clone` to clone the repository. The repository could be located
anywhere. If you have it set up on the same server as you're currently using,
inside the `repos` directory, you can clone it by passing `git clone` the path
to the repository.

```sh
git clone ~/repos/myrepo ~/apps/myrepo
```

Change into the repository.

```sh
cd ~/apps/myrepo
```

Install the project's dependencies.

```sh
npm install
```

Disable Next.js telemetry.

```sh
npx next telemetry disable
```

Build the project.

```sh
npm run build
```

Serve the project

```sh
npm start
```

By default, the app will be accessible on port 3000.

### Test the Application Server

Open a web browser and visit http://[your server's IP address/hostname]:3000.

If you're using a virtual machine, are working from the host machine, and you
have port 3000 forwarded, you can visit http://localhost:3000.

#### Forward the Port on VirtualBox

If you are using VirtualBox, you will need to forward port 3000.

Open **Oracle VM VirtualBox Manager**.

Click to highlight the server virtual machine.

Click on **Settings**.

Click **Network** in the pane on the left.

Under the **Adapter 1** tab, click **Advanced**.

Click **Port Forwarding**.

Click the **Adds new port forwarding rule.** button on the right. It looks like
a green diamond with a green "plus" sign on top of it.

Under name, enter **Node.js**.

Under **Host Port**, enter **3000**.

Under **Guest Port**, enter **3000**.

Note that you can change the **Host Port** to another number if you are serving
multiple apps on port **3000** on other virtual machines or port 3000 is
otherwise already in use.

With the port forwarded correctly, you should be able to visit
http://localhost:3000 and see the Next.js app.

You may wish to manage the Node.js process using PM2. See separate guide.
