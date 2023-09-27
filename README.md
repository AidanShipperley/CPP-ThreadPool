# CPP-ThreadPool

**An easy-to-use and efficient ThreadPool in C++11, with absolutely no dependencies.**

## How To Use

To use CPP-ThreadPool in your project, add this to your source code:

```C++
#include "ThreadPool.h"
#include <memory> // For unique pointer
```

To create the thread pool object on the heap, run this to define it:

```C++
std::unique_ptr<ThreadPool> threadPool;
// Here specify how many threads you want to use in the threadpool
threadPool = std::make_unique<ThreadPool>(std::thread::hardware_concurrency());
```

To start the thread pool, run start:

```C++
threadPool->start();
```

Now, we are ready to use the thread pool. You can add jobs easily to the pool with queuejob:
```C++
std::vector<int> my_vector(10);

for (int i = 0; i < 10; i++) {

  // Add job to pool
  threadPool->queueJob([this, i, &my_vector]() mutable {
    my_vector[i] = i;
  });

}

// Wait for all threads to finish
threadPool->waitForJobsToFinish();
```

You can also optionally query if the pool is busy with busy:
```C++
if (threadPool->busy()) {
  std::cout << "Pool has pending jobs!" << std::endl;
}
```


Finally, to stop the threadpool and deallocate all threads, run stop:
```C++
threadPool->stop();
```