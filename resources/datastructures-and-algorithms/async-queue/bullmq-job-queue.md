https://github.com/taskforcesh/bullmq/blob/master/src/classes/async-fifo-queue.ts#L13

This array-based (for now) async queue is the queue that bullMQ uses to schedule its jobs.

This async behavior [allows the main `run` method to wait non-blockingly](https://github.com/taskforcesh/bullmq/blob/fb7c64849dde45516ac8f1f313e242bae935fce8/src/classes/worker.ts#L375).


