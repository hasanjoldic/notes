# How To Scale Node.js Applications with Clustering

[DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-scale-node-js-applications-with-clustering)

When you run a Node.js program on a system with multiple CPUs, it creates a process that uses only a single CPU to execute by default. Since Node.js uses a single thread to execute your JavaScript code, all the requests to the application have to be handled by that thread running on a single CPU.

As a solution, Node.js introduced the `cluster` module, which creates multiple copies of the same application on the same machine and has them running at the same time. It also comes with a load balancer that evenly distributes the load among the processes using the round-robin algorithm. If a single instance crashes, users can be served by the remaining processes that are still running.

## `culster` node module

```js
import cluster from "cluster";
import os from "os";
import { dirname } from "path";
import { fileURLToPath } from "url";

const __dirname = dirname(fileURLToPath(import.meta.url));

const cpuCount = os.cpus().length;

cluster.setupPrimary({
  exec: __dirname + "/index.js",
});
```

## `pm2`

```bash
pm2 start index.js -i 0
```

The `-i` option accepts the number of worker processes you want pm2 to create. If you pass the argument `0`, pm2 will automatically create as many worker processes as there are CPUs on your machine.

