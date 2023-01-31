# What's new in TensorFlow 2.x


The following are all the changes coming in TensorFlow 2.x. Let's have a closer look at them:

* Eager Execution / tf.function
* Integration of the Keras API
* Facilitated distributed training
* TF Data
* TF SavedModel
* TensorFlow Hub
* TensorFlow Serving
* TensorFlow Lite
* TensorFlow.js
* Tidying up the API
* The conversion tool
* Alternative variable scoping
  

## Eager Execution


Lack of eager execution was one of the main complaints against TensorFlow. We all can relate. Having to execute the whole graph and then trying to debug based on the errors was very tedious. Especially, since values of intermediate results haven't been accessible without printing them out by mixing in debug statements into the production code.

With TensorFlow 2.0, eager execution is activated by default and the very cool thing is that the code nearly doesn't change. Under the hood, you are just working with so-called "EagerTensors" instead of "Tensors" but since they share the same interface, the difference is barely noticeable. Even in execution speed, the difference is hard to see. 

This means, from now on, TensorFlow code can be used and debugged as ordinary python code (using numpy for example). This is one aspect of making TensorFlow more pythonic.

Below there are two tasks. I highly recommend doing them because while watching me coding and coding yourself you'll definitely internalize the material.

### Tasks

1. [Watch me coding](https://www.youtube.com/watch?v=J3_b4461qxU)


2. [Code yourself](https://github.com/romeokienzler/TensorFlow/blob/master/notebooks/tf2.eagerexec.ipynb) 

## Integration of the Keras API

Actually, Keras is one of the greatest APIs on the planet for DeepLearning. Now Keras has been eaten up by TensorFlow. A bit sad, but in reality it doesn't make any difference since nearly everyone used Keras on top of TensorFlow anyway. So let's consider Keras to be part of TensorFlow (or TensorFlow to be part of Keras). The cool thing is, that you now can use the straightforward, and easy to use Keras API and still can claim to be a TensorFlow developer. Yeah, Google made Keras the official high level API of TensorFlow.

So you might think, so what? Just some imports change. But this is only one part of the story. Yes, the imports changed, and as you can see later in the example, you can basically leave your existing Keras code intact most of the times and just change the import and you are done.

But in addition, Keras now can make use of built-in TensorFlow functionality which wasn't possible before. For example, you can take your 1:1 Keras code and TensorFlow will scale it to a large GPU or TPU cluster. We'll have a look at this in the next chapter.

For now, just follow along the video and code exercise below to get an idea how things work:

### Tasks

1. [Watch me coding](https://www.youtube.com/watch?v=D4mJZQdgV0Y)


2. [Code yourself](https://github.com/romeokienzler/TensorFlow/blob/master/notebooks/tf2.keras.ipynb) 


Latest version TensorFlow 2.11.0

Breaking Changes

The tf.keras.optimizers.Optimizer base class now points to the new Keras optimizer, while the old optimizers have been moved to the tf.keras.optimizers.legacy namespace.

If you find your workflow failing due to this change, you may be facing one of the following issues:

Checkpoint loading failure. The new optimizer handles optimizer state differently from the old optimizer, which simplifies the logic of checkpoint saving/loading, but at the cost of breaking checkpoint backward compatibility in some cases. If you want to keep using an old checkpoint, please change your optimizer to tf.keras.optimizer.legacy.XXX (e.g. tf.keras.optimizer.legacy.Adam).

TF1 compatibility. The new optimizer, tf.keras.optimizers.Optimizer, does not support TF1 any more, so please use the legacy optimizer tf.keras.optimizer.legacy.XXX. We highly recommend migrating your workflow to TF2 for stable support and new features.

Old optimizer API not found. The new optimizer, tf.keras.optimizers.Optimizer, has a different set of public APIs from the old optimizer. These API changes are mostly related to getting rid of slot variables and TF1 support. Please check the API documentation to find alternatives to the missing API. If you must call the deprecated API, please change your optimizer to the legacy optimizer.

Learning rate schedule access. When using a tf.keras.optimizers.schedules.LearningRateSchedule, the new optimizer's learning_rate property returns the current learning rate value instead of a LearningRateSchedule object as before. If you need to access the LearningRateSchedule object, please use optimizer._learning_rate.

If you implemented a custom optimizer based on the old optimizer. Please set your optimizer to subclass tf.keras.optimizer.legacy.XXX. If you want to migrate to the new optimizer and find it does not support your optimizer, please file an issue in the Keras GitHub repo.

Errors, such as Cannot recognize variable.... The new optimizer requires all optimizer variables to be created at the first apply_gradients() or minimize() call. If your workflow calls the optimizer to update different parts of the model in multiple stages, please call optimizer.build(model.trainable_variables) before the training loop.

Timeout or performance loss. We don't anticipate this to happen, but if you see such issues, please use the legacy optimizer, and file an issue in the Keras GitHub repo.

The old Keras optimizer will never be deleted, but will not see any new feature additions. New optimizers (for example, tf.keras.optimizers.Adafactor) will only be implemented based on the new tf.keras.optimizers.Optimizer base class.

tensorflow/python/keras code is a legacy copy of Keras since the TensorFlow v2.7 release, and will be deleted in the v2.12 release. Please remove any import of tensorflow.python.keras and use the public API with from tensorflow import keras or import tensorflow as tf; tf.keras.

If you want to learn more, please have a look at our [book](https://learning.oreilly.com/library/view/whats-new-in/9781492073727/)

