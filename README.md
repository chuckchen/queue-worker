# Queue Worker for Async Tasks

This is a strip down implementation of async queue for long-running tasks, such as sending emails or sync data to R2/databases.

The basic flow is like below.

```mermaid
flowchart LR
	producer(producer)
	queue((Cloudflare\nQueue))
	consumer(consumer)
	job1(sending\nemails)
	job2(syncing\ndb)
	job3(other\ntask)

	subgraph worker [Cloudflare Workers]
		producer-->queue
		queue-->consumer
	end
	subgraph external [External]
		direction TB
		consumer-.->job1
		consumer-.->job2
		consumer-.->job3
	end
```

## Instructions

### Set up secrets

To add or update your Queue Auth Secret to can run the command below, or you can use the Cloudflare Web UI

`pnpm dlx wrangler secret put QUEUE_AUTH_SECRET`

### Deploy worker

to deploy the worker, run the command below.

`pnpm deploy`

If you found the queue not found error you need to create a queue name `my-queue` on your Cloudflare.

