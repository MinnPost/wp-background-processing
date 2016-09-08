# WP Background Processing

WP Background Processing can be used to fire off non-blocking asynchronous requests or as a background processing tool, allowing you to queue tasks. Check out the [example plugin](https://github.com/A5hleyRich/wp-background-processing-example) or read the [accompanying article](https://deliciousbrains.com/wordpress-background-processing/).

Inspired by [TechCrunch WP Asynchronous Tasks](https://github.com/techcrunch/wp-async-task).

__Requires PHP 5.4.__

### Async Request

Async requests are useful for pushing slow one-off tasks such as sending emails to a background process. Once the request has been dispatched it will process in the background instantly.

Extend the `WP_Job` class:

```
<?php
class WP_Example_Job extends WP_Job {
    /**
     * @var string
     */
    protected $param1;

    /**
     * @var string
     */
    protected $param2;
    /**
     * WP_Example_Job constructor.
     *
     * @param $name
     */
    public function __construct( $param1, $param2 ) {
        $this->param1 = $param1;
        $this->param2 = $param2;
    }

    /**
     * Handle
     *
     * Override this method to perform any actions required on each
     * queue item. Return the modified item for further processing
     * in the next pass through. Or, return false to remove the
     * item from the queue.
     *
     * @param mixed $item Queue item to iterate over
     *
     * @return mixed
     */
    public function handle() {
        // Process the job
    }
}
```

The only required method is `handle` which performs the queue logic. You can pass optional data to the `__constructor`, which is then available in your `handle` method for processing.

Jobs are pushed to the queue using the helper function wp_queue.

```
wp_queue( new WP_Example_Job( $param1, $param2 ) );
```

That's all there is to it. There's no longer any need to dispatch the job as it's taken care of by the HTTP worker.